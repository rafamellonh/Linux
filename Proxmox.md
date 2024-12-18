## Install Wifi 
```
* apt install wpasupplicant wireless-tools -y
* 
ip link set wlp2s0 up

ip link show wlp2s0

iwlist wlp2s0 scan | grep ESSID

sudo wpa_passphrase "SSID" "PASSWORD" > /etc/wpa_supplicant/wpa_supplicant.conf

wpa_supplicant -B -i wlp2s0 -c /etc/wpa_supplicant/wpa_supplicant.conf

dhclient wlp2s0

nano /etc/network/interfaces

auto wlp2s0

iface wlp2s0 inet dhcp

    wpa-conf /etc/wpa_supplicant/wpa_supplicant.conf
	
systemctl restart networking


## Por que o Wi-Fi não funciona diretamente em um bridge?
A interface Wi-Fi (wlp2s0) não pode ser adicionada diretamente a um bridge tradicional no Linux. Isso ocorre porque o padrão 802.11 não suporta múltiplos endereços MAC conectados diretamente (como ocorre com VMs em uma bridge).
Para resolver isso, você pode usar o recurso masquerading (NAT) ou configurar um Access Point (AP).
1. Configurar Wi-Fi para VMs com NAT
A solução mais simples é criar uma interface virtual (bridge ou NAT) no Proxmox e encaminhar o tráfego para o Wi-Fi com NAT.

a) Configuração do /etc/network/interfaces
Ajuste sua configuração para que o wlp2s0 funcione como interface de saída com NAT para as VMs:

plaintext
Copiar código
auto lo
iface lo inet loopback

iface enp1s0 inet manual

auto wlp2s0
iface wlp2s0 inet static
        address 192.168.1.175/24
        gateway 192.168.1.254
        post-up echo 1 > /proc/sys/net/ipv4/ip_forward
        post-up iptables -t nat -A POSTROUTING -o wlp2s0 -j MASQUERADE
        post-up iptables -A FORWARD -i vmbr0 -o wlp2s0 -j ACCEPT
        post-up iptables -A FORWARD -i wlp2s0 -o vmbr0 -j ACCEPT

auto vms
iface vms inet static
        address 192.168.100.1/24
        bridge-ports none
        bridge-stp off
        bridge-fd 0
Aqui está o que acontece:

vmbr0: É configurada como uma bridge sem portas físicas (bridge "local").
As VMs serão configuradas para usar vmbr0 como rede, com IPs na faixa 10.0.0.x/24.
O tráfego será mascarado



## Install dnsamasq para o dhcp nas vms

Sim, é possível configurar para que as VMs obtenham IPs automaticamente (via DHCP) usando a configuração da Etapa 1 (NAT). Para isso, você precisará adicionar um servidor DHCP no host Proxmox para atribuir os IPs às VMs conectadas ao bridge vmbr0.

Aqui está como fazer isso:

1. Instalar e configurar o dnsmasq
O dnsmasq é uma ferramenta leve que combina um servidor DHCP e DNS, ideal para este tipo de configuração.

a) Instalar o dnsmasq
No Proxmox, instale o dnsmasq:

bash
Copiar código
apt update
apt install dnsmasq
b) Configurar o dnsmasq
Edite ou crie o arquivo de configuração em /etc/dnsmasq.d/vmbr0.conf:

plaintext
Copiar código

interface=vmbr0                  # Interface usada pelo DHCP
bind-interfaces                  # Vincular apenas a essa interface
dhcp-range=192.168.100.100,192.168.100.200,12h  # Faixa de IPs e tempo de lease
dhcp-option=3,192.168.100.1      # Gateway (IP do host no vmbr0)
dhcp-option=6,8.8.8.8,8.8.4.4    # Servidores DNS (Google DNS)


c) Reiniciar o dnsmasq
Após configurar, reinicie o serviço para aplicar as alterações:

bash
Copiar código
systemctl restart dnsmasq
d) Verificar se o dnsmasq está ativo
Certifique-se de que o dnsmasq está funcionando:

bash
Copiar código
systemctl status dnsmasq

```
