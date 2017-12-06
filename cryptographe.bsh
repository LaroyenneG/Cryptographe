#!/bin/bash
#===============================================================================
#
#          FILE:  cryptographe.bsh
#
#         USAGE:  ./cryptographe.bsh [OPTION] [PATH]
#
#   DESCRIPTION: Recursive cryptographegraphe
#
#       OPTIONS:  -c -d
#  REQUIREMENTS:  bash openssl
#          BUGS:  ---
#         NOTES:  ---
#        AUTHOR:  LAROYENNE Guillaume
#       COMPANY:  ---
#       VERSION:  1.0
#       CREATED:  5/12/2017
#      REVISION:  
#===============================================================================


MOD_OP=0;
CRYPT_FILE_EXT=".enc";
PASSWORD="";
BASE="-base64"; 


function recursiveSearch() {

    if [[ $# -ne 1 ]]; then
        echo "[ANOMALY] invalid argument number";
        exit;
    fi
 
    for file in "$1"/* ; do
 
        if [[ -d "$file" ]]; then
            recursiveSearch "$file";
        fi

    	if [[ "$MOD_OP" -eq "0" ]]; then
    		crypt "$file";
    	else
    		decrypt "$file";
    	fi
    	
    done
}


function crypt() {

    if [[ $# -ne 1 ]]; then
        echo "[ANOMALY] invalid argument number";
        exit;
    fi

    errors=$(openssl enc "$BASE" -in "$1" -out "$1$CRYPT_FILE_EXT" -pass "pass:$PASSWORD" 2>&1 >/dev/null);

    if [[ "$errors" != "" ]]; then
        echo "cannot crypt file $1" >&2;
        exit;
    fi

    if [[ -e "$1$CRYPT_FILE_EXT" ]]; then
        rm "$1";
    fi
}

function decrypt() {

    if [[ $# -ne 1 ]]; then
        echo "[ANOMALY] invalid argument number";
        exit;
    fi

    echo "decrypt";
}



if [[ $# -lt 2 ]]; then
	echo "Uage : cryptographe [OPTION] [PATH]";
	exit;
fi

case "$1" in
    "-c")   MOD_OP=0;
        ;;
    "-d")   MOD_OP=1;
        ;;
    *)      echo "$1, invalid option";
            exit;
        ;;
esac

if [ ! -e "$2" ] && [ ! -d "$2" ]; then
    echo "invalid argument, $2 is not a file or directory";
    exit;
fi

if [[ "$MOD_OP" -eq "0" ]]; then
    echo "You can enter a key :";
    read -s inppass1;
    echo "Retype the key please :";
    read -s inppass2;

    if [[ "$inppass1" != "$inppass2" ]]; then
        echo "Typing error";
        exit;
    fi
    PASSWORD=$inppass1;
else
    echo "You can enter the key :";
    read -s inppass;
    PASSWORD=$inppass;
fi

echo "$PASSWORD";

recursiveSearch $2;