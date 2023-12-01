#! /bin/bash

PROG=$(basename $0)
SRC=rafael@192.168.2.61:/dados/
DST=/dados/
LOCKFILE=/tmp/$PROG.lock
LOGFILE=/var/log/$PROG.log

# Redirect all output to log file
exec >>"$LOGFILE" 2>&1

echo $(date +%Y-%m-%d\ %T) START $PROG

if [ -f "$LOCKFILE" ]
then
        echo "*** LOCKFILE $LOCKFILE FOUND - ABORTING ***"
        exit 1
else
        touch "$LOCKFILE"
fi

/usr/bin/rsync -rltDvau --delete --exclude=php/twig/ --exclude=css/ --exclude=js/ --exclude=js/optimized/ --exclude=synonyms/ --exclude=styles/ --exclude=404_pages/ --exclude=403_pages/ "$SRC/" "$DST/"

rm -f "$LOCKFILE"

echo $(date +%Y-%m-%d\ %T) END   $PROG
exit 0
