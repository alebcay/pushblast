#!/usr/bin/env bash

if [[ $1 = "" ]] ; then
  echo "usage: pushblast \"command_to_be_executed\""
  exit 1
fi

if [ ! -d ~/.config/pushblast/lib ] ; then
  mkdir -p ~/.config/pushblast/lib
fi

if [ ! -e ~/.config/pushblast/lib/pushbullet ] ; then
  echo "Downloading helper file 1 of 1:"
  curl -# https://raw.githubusercontent.com/Red5d/pushbullet-bash/master/pushbullet > ~/.config/pushblast/lib/pushbullet
fi

chmod +x ~/.config/pushblast/lib/pushbullet

if [ ! -e ~/.config/pushblast/pushblastrc ] ; then
  touch ~/.config/pushblast/pushblastrc
  echo "Welcome to PushBlast! Before I get started, I'll need a few things from you regarding your PushBullet account."
  echo -e "\nPlease provide your PushBullet API key, also known as an access token. I need this to be able to push stuff: "
  read API_KEY
  echo -e "\nPlease provide the device name you wish to push to. Device names are case sensitive, so be careful: "
  read DEV_NAME
  echo -e "\nPlease name this computer. Computer names are shown on the notification so you know where a notification came from."
  read COMP_NAME
  echo "API_KEY=\"$API_KEY\"" >> ~/.config/pushblast/pushblastrc
  echo "DEV_NAME=\"$DEV_NAME\"" >> ~/.config/pushblast/pushblastrc
  echo "COMP_NAME=\"$COMP_NAME\"" >> ~/.config/pushblast/pushblastrc
  echo -e "\nLooks like we're all good to go. I'll go ahead and run the command you gave."
fi

source ~/.config/pushblast/pushblastrc

if [[ $API_KEY = "" ]] ; then
  echo "Invalid API key. Find your access token at https://www.pushbullet.com/account."
  exit 1
fi

touch ~/.config/pushbullet
echo "API_KEY=$API_KEY" > ~/.config/pushbullet

if [[ $DEV_NAME = "" ]] ; then
  echo "Invalid target device name. Find your list of devices at https://www.pushbullet.com."
  exit 1
fi

if [[ $COMP_NAME = "" ]] ; then
  echo "Invalid computer name. Enter a name for this computer (it does not have to be unique or in use with Pushbullet)."
  exit 1
fi

(~/.config/pushblast/lib/pushbullet list | grep $DEV_NAME) >/dev/null 2>&1

if [ $? != 0 ] ; then
  echo "The device $DEV_NAME does not exist or is unusable."
  exit 1
fi

EXECUTE=$1
$1
EXIT=$?

~/.config/pushblast/lib/pushbullet push $DEV_NAME note "The task \"$EXECUTE\" on \"$COMP_NAME\" finished with exit code $EXIT." >/dev/null 2>&1
exit $EXIT
