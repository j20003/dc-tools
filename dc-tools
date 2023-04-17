#!/bin/bash -x
#Juan  https://j20003.es
HEIGHT=20
WIDTH=50
CHOICE_HEIGHT=10
BACKTITLE="Tools Domain"
TITLE="Domain Config, choose option"
MENU="\nChoose one of the following options:"

OPTIONS=(1 "Change Password Options of Domain"
 2  "List Group DC"
 3  "Add New Group Domain"
 4  "Remove Group Domain"
 5  "Add Members to Group DC"
 6  "Remove Members to Group DC"
 7  "Add New User DC"
 8  "Del User DC"
 9  "Check which users have a group"
 10 "Check which groups a user belongs to"
 11 "Add New Net Share to Domain"
 12 "Delete net Share in Domain"
 13 "List Computer Of Domain"
 14 "Delete Computer Of Domain"
 15 "Show Net Shares"
 16 "Delete files"
 17 "Make a User a DC Administrator"
 18 "Edit file smb.conf"
 19 "Shutdown"
 20 "Restart"
 21 "Exit")


CHOICE=$(dialog --clear \
                --backtitle "$BACKTITLE" \
                --title "$TITLE" \
                --menu "$MENU" \
                $HEIGHT $WIDTH $CHOICE_HEIGHT \
                "${funcheck[@]}" \
		"${OPTIONS[@]}" \
		2>&1 >/dev/tty)




clear
case $CHOICE in

	1)
 #####################Change password options of Domain#################################
exec 3>&1

# Store data to $VALUES variable
VALUES=$(dialog --ok-label "Submit" \
	  --backtitle "" \
	  --title "Default Settings Passwords" \
	  --form "\nCan Change this Settings Passwords" \
0 0 0 \
	"Define_complexity?"            1 1 "off" 1 26 6 0 \
	"Change_password_history"       2 1 "3"   2 26 6 0 \
	"min_password_character"        3 1 "4"   3 26 6 0 \
	"min_password_valid time"       4 1 "0"   4 26 6 0 \
	"password_expiration_time"      5 1 "0"   5 26 6 0 \
2>&1 1>&3)

exec 3>&-

complexity=$(echo "$VALUES" | sed -n 1p)
historylength=$(echo "$VALUES" | sed -n 2p)
minpwdlength=$(echo "$VALUES" | sed -n 3p)
minpwdage=$(echo "$VALUES" | sed -n 4p)
maxpwdage=$(echo "$VALUES" | sed -n 5p)

samba-tool domain passwordsettings set --complexity=$complexity
samba-tool domain passwordsettings set --history-length=$historylength
samba-tool domain passwordsettings set --min-pwd-length=$minpwdlength
samba-tool domain passwordsettings set --min-pwd-age=$minpwdage
samba-tool domain passwordsettings set --max-pwd-age=$maxpwdage
smbcontrol all reload-config

#samba-tool domain passwordsettings show

passwordset=$(samba-tool domain passwordsettings show)

	(dialog --title "Settings Passwords" \
	--stdout \
	--msgbox "$passwordset" 0 0 )

dc-tools
 ;;

        2)
 #######################List Group DC########################
 group=$(samba-tool group list)
	(dialog --title "Users list" \
	--stdout \
	--msgbox "$group" 0 0 )
dc-tools
;;


        3)
 ######################Add New Group DC###############################
  answer=$(dialog --title "Add New Group DC"                  \
                   --separate-widget $"\n"               \
                   --form  ""         \
                   0 0 0                                 \
                   "Name:"   1 1 "$name" 1 10 20 0 \
		   3>&1 1>&2 2>&3 3>&-)

name=$(echo "$answer" | sed -n 1p)
samba-tool group add $name &&

smbcontrol all reload-config
dc-tools
 ;;

	4)
########################Remove Group Domain##############################

rm -r groupdc
mkdir groupdc
cd groupdc

samba-tool group list > groupdc

sed -i 's/Performance Monitor Users//g' groupdc
sed -i 's/Remote Desktop Users//g' groupdc
sed -i 's/Cryptographic Operators//g' groupdc
sed -i 's/Allowed RODC Password Replication Group//g' groupdc
sed -i 's/Domain Computers//g' groupdc
sed -i 's/Cert Publishers//g' groupdc
sed -i 's/Pre-Windows 2000 Compatible Access//g' groupdc
sed -i 's/Incoming Forest Trust Builders//g' groupdc
sed -i 's/Backup Operators//g' groupdc
sed -i 's/Domain Admins//g' groupdc
sed -i 's/DnsUpdateProxy//g' groupdc
sed -i 's/RAS and IAS Servers//g' groupdc
sed -i 's/Schema Admins//g' groupdc
sed -i 's/DnsAdmins//g' groupdc
sed -i 's/Domain Guests//g' groupdc
sed -i 's/Certificate Service DCOM Access//g' groupdc
sed -i 's/Enterprise Admins//g' groupdc
sed -i 's/Terminal Server License Servers//g' groupdc
sed -i 's/Administrators//g' groupdc
sed -i 's/Distributed COM Users//g' groupdc
sed -i 's/Network Configuration Operators//g' groupdc
sed -i 's/Group Policy Creator Owners//g' groupdc
sed -i 's/Print Operators//g' groupdc
sed -i 's/Event Log Readers//g' groupdc
sed -i 's/Denied RODC Password Replication Group//g' groupdc
sed -i 's/Performance Log Users//g' groupdc
sed -i 's/Replicator//g' groupdc
sed -i 's/IIS_IUSRS//g' groupdc
sed -i 's/Account Operators//g' groupdc
sed -i 's/Enterprise Read-only Domain Controllers//g' groupdc
sed -i 's/Server Operators//g' groupdc
sed -i 's/Domain Users//g' groupdc
sed -i 's/Read-only Domain Controllers//g' groupdc
sed -i 's/Windows Authorization Access Group//g' groupdc
sed -i 's/Guests//g' groupdc
sed -i 's/Users//g' groupdc
sed -i 's/Domain Controllers//g' groupdc


cat groupdc | sed '/^ *$/d' > groupdc00
mv groupdc00 groupdc

cp  groupdc groupdc-b

delgpr=$(cat groupdc)
                (dialog --title "These are the groups you can delete" \
                --stdout \
                --msgbox "$delgpr" 0 0 )


###comando final seleccion grupo##

######primera parte
sed '/./=' groupdc | sed '/./N; s/\n/ /' > groupdc00
mv groupdc00 groupdc
sed -i 's/$/ off/' groupdc

touch  delgroupdc
chmod +x delgroupdc


######Segunda parte
sed -i "s/^/'/" groupdc-b
sed -i "s/$/'/" groupdc-b
sed '/./=' groupdc-b | sed '/./N; s/\n/) samba-tool group delete /' > groupdc-b01
sed '/./=' groupdc-b | sed '/./N; s/\n/ /' > groupdc-b1
sed 'a\;;' groupdc-b01 > groupdc-b02

###comando final seleccion grupo##
echo '#!/bin/bash -x' >> delgroupdc
echo 'funcheck=(dialog --separate-output --buildlist "Selet Group to Delete" 0 0 0)' >> delgroupdc
echo 'opciones=(' >> delgroupdc
cat groupdc >> delgroupdc
echo ')' >> delgroupdc
#####################################

echo 'selecciones=$("${funcheck[@]}" "${opciones[@]}" 2>&1 >/dev/tty)' >> delgroupdc
echo 'for seleccion in $selecciones' >> delgroupdc
echo 'do' >> delgroupdc
echo 'case $seleccion in' >> delgroupdc


cat groupdc-b02 >> delgroupdc


echo 'esac' >> delgroupdc
echo 'done' >> delgroupdc

#######################################################################

#while read group
#do
#samba-tool group delete ${group}
#done < groupdc
./delgroupdc
cd ..
rm -r groupdc
dc-tools

 ;;

	5)

 #######################Add members to group dc###############################
rm -r userdc/

mkdir userdc
cd userdc

samba-tool group list > group
sed 's/[A-Z]/\L&/g' group > group1
sed 's/ /\\ /g' group > group2
mv group2 group
sed -i 's/$/ ""/' group
#sed -i 's/^/"/' group
#sed '/./=' group | sed '/./N; s/\n/ /' > groupd00
#sed '/./=' group | sed '/./N; s/\n/) echo /' > group01
sed 'a\;;' group01 > group02
mv group02 group01
sed -i 's/$/ \\/' group
touch  domaingrp
chmod +x domaingrp

###comando seleccion grupo##
echo '#!/bin/bash -x' >> domaingrp
echo 'grupo=$(dialog --title "Selecionar grupo" --stdout --menu "Opciones" 0 0 0 \' >> domaingrp
cat group >> domaingrp
echo 'no\ selection "" )' >> domaingrp
echo 'samba-tool group listmembers "${grupo}"' >> domaingrp

###########Pasar al segundo ejecutable para cojer la variable#######################
cd ..
cp listuser userdc/
cd userdc
cat listuser >> domaingrp
./domaingrp
cd ..
rm -r userdc/

dc-tools

 ;;

	6)
######################Remove members from group dc################################
rm -r userdc/

mkdir userdc
cd userdc

samba-tool group list > group
sed 's/[A-Z]/\L&/g' group > group1
sed 's/ /\\ /g' group > group2
mv group2 group
sed -i 's/$/ ""/' group
#sed -i 's/^/"/' group
#sed '/./=' group | sed '/./N; s/\n/ /' > groupd00
#sed '/./=' group | sed '/./N; s/\n/) echo /' > group01
sed 'a\;;' group01 > group02
mv group02 group01
sed -i 's/$/ \\/' group
touch  domaingrp
chmod +x domaingrp

###comando final seleccion grupo##
echo '#!/bin/bash -x' >> domaingrp
echo 'grupo=$(dialog --title "Selecionar grupo" --stdout --menu "Opciones" 0 0 0 \' >> domaingrp
cat group >> domaingrp
echo 'no\ selection "" )' >> domaingrp

echo 'samba-tool group listmembers "${grupo}"' >> domaingrp


###########Pasar al segundo ejecutable para cojer la variable#######################
cd ..
cp listuserdel userdc/
cd userdc
cat listuserdel >> domaingrp
./domaingrp
cd ..
rm -r userdc/


dc-tools
 ;;

	7)
########################Add New  User DC##############################
hostname=$(hostname)

  answer=$(dialog --title "Add New User Domain"                  \
                   --separate-widget $"\n"               \
                   --form  "In section Domain Write:\nIP: xx:xx:xx:xx\nName of server domain, example: debiandc\nOr name of domain, example: versalles.local\nDefault machine´s hostname\nVolume HDD default 500M, will be multiplied for 4\nVolume ONLY numbers"         \
                   0 0 0                                 \
                   "Name:"   1 1 "$name" 1 10 20 0 \
                   "Password:" 2 1 "versalles" 2 10 20 0 \
                   "Surname:"    3 1 "Domain user" 3 10 25 0 \
                   "Domain:"    4 1 "$hostname" 4 10 30 0 \
	           "Volume:"    5 1 "512" 5 10 20 0 \
		   3>&1 1>&2 2>&3 3>&-)

name=$(echo "$answer" | sed -n 1p)
password=$(echo "$answer" | sed -n 2p)
surname=$(echo "$answer" | sed -n 3p)
domain=$(echo "$answer" | sed -n 4p)
domain=$(echo "$answer" | sed -n 4p)
volume=$(echo "$answer" | sed -n 5p)


name3="admin"
name2=""
usersdc=$(samba-tool user list | grep $name )


if [ $name = $name2 ]

        then
		(dialog --title "ERROR" \
		--stdout \
		--msgbox "Variable is empty\nWrite a name valid" 0 0 )

elif [ $name = $name3 ]

        then
                (dialog --title "ERROR" \
                --stdout \
                --msgbox "Name not permit \nWrite a new name" 0 0 )


elif [ $name = $usersdc ]

        then
		(dialog --title "ERROR" \
		--stdout \
		--msgbox "The user exist \nWrite a new name" 0 0 )

else

    mkdir /home/users/$name
    chmod 700 /home/users/$name

############################################
    dd if=/dev/zero of=/media/usersdc/$name.img bs=${volume}M count=4
    mkfs.ext4 /media/usersdc/$name.img
    mount -o loop /media/usersdc/$name.img /home/users/$name
    echo >> /etc/fstab
    echo >> /etc/fstab "#1"$name
    echo >> /etc/fstab /media/usersdc/$name.img /home/users/$name ext4 loop 0 2
    echo >> /etc/fstab "#2"$name
###########################################

    samba-tool user create $name $password --given-name="$name" --surname="$surname"
    echo >> /etc/samba/smb.conf
    echo >> /etc/samba/smb.conf "#1"$name
    echo >> /etc/samba/smb.conf [$name]
    echo >> /etc/samba/smb.conf "browseable = no"
    echo >> /etc/samba/smb.conf "path = /home/users/"$name
    echo >> /etc/samba/smb.conf "read only = no"
    echo >> /etc/samba/smb.conf "admin users = "$name
    echo >> /etc/samba/smb.conf "#2"$name
    pdbedit $name -D Y:
    pdbedit $name -h \\\\$domain\\$name
    smbcontrol all reload-config

    var1=$(pdbedit $name -v)
    	(dialog --title "User of Domain added verbose" \
	--stdout \
	--msgbox "$var1" 0 0 )

fi

dc-tools
;;


        8)
 #######################Del User DC###############################
rm -r userdc
mkdir userdc
cd userdc

samba-tool user list > dcusers00.txt
cat dcusers00.txt
sleep 5
sed -i "s/Administrator//g" dcusers00.txt
sed -i "s/krbtgt//g" dcusers00.txt
sed -i "s/Guest//g" dcusers00.txt
cat dcusers00.txt
cat dcusers00.txt | sed '/^ *$/d' > dcusers.txt
rm dcusers00.txt
cat dcusers.txt
sleep 5
############################################################
###############################################################
mv dcusers.txt groupdc

cp  groupdc groupdc-b

delgpr=$(cat groupdc)
                (dialog --title "Users that can be removed" \
                --stdout \
                --msgbox "$delgpr" 0 0 )


###comando final seleccion grupo##

######primera parte
sed '/./=' groupdc | sed '/./N; s/\n/ /' > groupdc00
mv groupdc00 groupdc
sed -i 's/$/ off/' groupdc

touch  deluserdc
chmod +x deluserdc


######Segunda parte
sed -i "s/^/'/" groupdc-b
sed -i "s/$/'/" groupdc-b
sed '/./=' groupdc-b | sed '/./N; s/\n/) echo /' > groupdc-b01
sed -i 's/$/>> users000/' groupdc-b01
sed '/./=' groupdc-b | sed '/./N; s/\n/ /' > groupdc-b1
sed 'a\;;' groupdc-b01 > groupdc-b02

###comando final seleccion grupo##
echo '#!/bin/bash -x' >> deluserdc
echo 'funcheck=(dialog --separate-output --buildlist "Selet user to delete" 0 0 0)' >> deluserdc
echo 'opciones=(' >> deluserdc
cat groupdc >> deluserdc
echo ')' >> deluserdc
#####################################

echo 'selecciones=$("${funcheck[@]}" "${opciones[@]}" 2>&1 >/dev/tty)' >> deluserdc
echo 'for seleccion in $selecciones' >> deluserdc
echo 'do' >> deluserdc
echo 'case $seleccion in' >> deluserdc


cat groupdc-b02 >> deluserdc


echo 'esac' >> deluserdc
echo 'done' >> deluserdc
./deluserdc


################################################################
################################################################


while read user
do

                smbpasswd -x ${user}
		umount /home/users/${user}
                rm -r /home/users/${user}
		rm -r /media/usersdc/${user}.img
                #########delete records in samba & fstab files###################
                sed -i "/"#1"${user}/,/"#2"${user}/d" /etc/samba/smb.conf
		sed -i "/"#1"${user}/,/"#2"${user}/d" /etc/fstab
                sed -i "s/", "${user}//g" /etc/samba/smb.conf
                sed -i "s/${user}","//g" /etc/samba/smb.conf
                sed -i "s/${user}//g" /etc/samba/smb.conf

done < users000

cd ..
rm -r userdc
dc-tools
;;



   9)
##############Check which users have a group###############
rm -r grouplist/
mkdir grouplist
cd grouplist

samba-tool group list > group
sed 's/[A-Z]/\L&/g' group > group1
sed 's/ /\\ /g' group > group2
mv group2 group
sed -i 's/$/ ""/' group
#sed -i 's/^/"/' group
#sed '/./=' group | sed '/./N; s/\n/ /' > groupd00
#sed '/./=' group | sed '/./N; s/\n/) echo /' > group01
sed 'a\;;' group01 > group02
mv group02 group01
sed -i 's/$/ \\/' group
touch  domaingrp
chmod +x domaingrp

###comando final seleccion grupo##
echo '#!/bin/bash -x' >> domaingrp
echo 'grupo=$(dialog --title "Selec Group" --stdout --menu "Opciones" 0 0 0 \' >> domaingrp
cat group >> domaingrp
echo 'no\ selection "" )' >> domaingrp


echo 'respuesta=$(samba-tool group listmembers "${grupo}")' >> domaingrp

echo '(dialog --title "Members Of $grupo" --stdout --msgbox "$respuesta" 0 0 )' >> domaingrp
./domaingrp
cd ..
rm -r grouplist/
dc-tools
;;


  10)
#################Check which groups a user belongs to################
rm -r userlist/
mkdir userlist
cd userlist

samba-tool user list > user
sed 's/[A-Z]/\L&/g' user > user1
sed 's/ /\\ /g' user > user2
mv user2 user
sed -i 's/$/ ""/' user
sed 'a\;;' user01 > user02
mv user02 user01
sed -i 's/$/ \\/' user
touch  usergrp
chmod +x usergrp

###comando final seleccion grupo##
echo '#!/bin/bash -x' >> usergrp
echo 'user=$(dialog --title "Selec User" --stdout --menu "Opciones" 0 0 0 \' >> usergrp
cat user >> usergrp
echo 'no\ selection "" )' >> usergrp


echo 'respuesta=$(samba-tool user getgroups "${user}")' >> usergrp

echo '(dialog --title "Groups to which the user $user belongs" --stdout --msgbox "$respuesta" 0 0 )' >> usergrp
./usergrp
cd ..
rm -r userlist/
dc-tools
;;

   11)
 #######################Add new share in Domain###############################
   share=$(dialog --title "" \
                   --stdout \
                   --inputbox "Name New Share?" 0 0)

	var1=$share
	var2=""
	var3=$(ls /home/shares | grep $share)

if [ $var1 = $var2 ]

        then
		(dialog --title "ERROR" \
		--stdout \
		--msgbox "Variable is empty\nWrite a name valid" 0 0 )

elif [ $var3 = $var1 ]

	then
		 (dialog --title "ERROR" \
		--stdout \
		--msgbox "Net share exists\nGive it a different name" 0 0 )
                dc-tools

	else

  admin=$(dialog --title "" \
                   --stdout \
                   --inputbox "Admin Group or User\nExamples:\nUser: madrid\nGroups: @profesores\nTo add Several users or groups, edit file like root or sudo user\nadd in line admin user\nUsers: admin users = madrid, caceres\nGroups: admin users = @profesores, @comun\nGroups and users: admin users = @profesores, @comun, madrid, caceres" 0 0)


varuser1=""
varuser2=$(samba-tool user list | grep $admin)

       if [ $varuser2 = $varuser1 ]

        then
		(dialog --title "ERROR" \
		--stdout \
		--msgbox "Variable is empty.\nOr the user no exists\nWrite a name valid" 0 0 )

	else

volume=$(dialog --title "" \
                   --stdout \
                   --inputbox "Volume to the new share" 0 0 512)

    mkdir /home/shares/$share
    chmod 700 /home/shares/$share


############################################
    dd if=/dev/zero of=/home/shares/$share.img bs=${volume}M count=4
    mkfs.ext4 /home/shares/$share.img
    mount -o loop /home/shares/$share.img /home/shares/$share
    echo >> /etc/fstab
    echo >> /etc/fstab "#1"$share
    echo >> /etc/fstab /home/shares/$share.img /home/shares/$share ext4 loop 0 2
    echo >> /etc/fstab "#2"$share
############################################

    echo >> /etc/samba/smb.conf
    echo >> /etc/samba/smb.conf "#1"$share
    echo >> /etc/samba/smb.conf ""[$share]
    echo >> /etc/samba/smb.conf "path = /home/shares/"$share
    echo >> /etc/samba/smb.conf "read only = no"
    echo >> /etc/samba/smb.conf "admin users = "$admin
    echo >> /etc/samba/smb.conf "#2"$share
    chmod 700 /home/shares/$share

    smbcontrol all reload-config

    varshare=$(smbclient -L localhost -U%)
	(dialog --title "List net disk Shares" \
	--stdout \
	--msgbox "$varshare" 0 0 )
	fi
fi
 dc-tools
;;

	12)
 #######################Delete net Share in Domain###############################

sharedelete=$(dialog --title "Selet net share to delete" \
                   --stdout \
                   --dselect /home/shares/.  14 70 )

var1=$(basename $sharedelete)

if [ $sharedelete = /home/shares/. ]

        then
		(dialog --title "ERROR" \
		--stdout \
		--msgbox "\nNo net share floder selected\nTry again" 0 0 )

elif [ $sharedelete = /home/shares/.. ]

        then
		(dialog --title "ERROR" \
		--stdout \
		--msgbox "\nNo net share floder selected\nTry again" 0 0 )


else

		(dialog --title "Are you sure to erase net share?" \
		--msgbox "\nIF YOU PRESS ENTER\n\n 
WILL DEFINITELY ELIMINATE\n\nTHE NETWORK SHARED DIRECTORY CALLED $sharedelete\n\nTo cacel Ctrl+x" 0 0 )

umount ${sharedelete}
rm -r $sharedelete
rm -r ${sharedelete}.img

    #########delete records in samba & fstab files###################
                sed -i "/"#1"${var1}/,/"#2"${var1}/d" /etc/samba/smb.conf
                sed -i "/"#1"${var1}/,/"#2"${var1}/d" /etc/fstab
                sed -i "s/", "${var1}//g" /etc/samba/smb.conf
                sed -i "s/${var1}","//g" /etc/samba/smb.conf
                sed -i "s/${var1}//g" /etc/samba/smb.conf


        (dialog --msgbox "The net share ${varshare1} was erased" 0 0)
fi
dc-tools
;;


	13)
 ######################List Computer Of Domain################################
#samba-tool computer list
#
computer=$(samba-tool computer list)
	(dialog --title "computer list" \
	--stdout \
	--msgbox "$computer" 0 0 )
dc-tools
 ;;

	14)
 ######################Delete Computer Of Domain################################

rm -r computerlist/
mkdir computerlist
cd computerlist

samba-tool computer list > computer
sed 's/[A-Z]/\L&/g' computer > computer1
sed 's/ /\\ /g' computer > computer2
sed 's/\$//g' computer2 > computer3
mv computer3 computer
cat computer
sed -i 's/$/ ""/' computer
sed -i 's/$/ \\/' computer
touch  computergrp
chmod +x computergrp

###comando final seleccion grupo##
echo '#!/bin/bash -x' >> computergrp
echo 'computer=$(dialog --title "Selec Computer" --stdout --menu "Opciones" 0 0 0 \' >> computergrp
cat computer >> computergrp
echo 'no\ selection "" )' >> computergrp


echo '(dialog --title "The Computer That Will Be Eliminated Is:" --stdout --msgbox "$computer" 0 0 )' >> computergrp
echo 'samba-tool computer delete "$computer"' >> computergrp
./computergrp
cd ..
rm -r computerlist/
dc-tools
;;


	15)
#######################Show Net Shares###############################
    varshare=$(smbclient -L localhost -U%)
	(dialog --title "List net disk Shares" \
	--stdout \
	--msgbox "$varshare" 0 0 )
dc-tools
;;


	16)
    ######################Browse and select files to delete################################

filedelete=$(dialog --title "Choose a file" \
                   --stdout \
                   --fselect $HOME/  14 70)
if [ -f "$filedelete" ]
then
    dialog --title "File erased" \
           --yesno "Are you sure to want delete the file called $filedelete?" 0 0
    ans=$?
    if [ $ans -eq 0 ]
    then
        rm "$filedelete"
        dialog --msgbox "El archivo $filedelete fue borrado" 0 0
    fi
fi
dc-tools
 ;;

	17)
#################Make a user a domain administrator###################

rm -r userlistadm/
mkdir userlistadm
cd userlistadm

samba-tool user list > user
sed 's/[A-Z]/\L&/g' user > user1
sed 's/ /\\ /g' user > user2
mv user2 user
sed -i 's/$/ ""/' user
sed 'a\;;' user01 > user02
mv user02 user01
sed -i 's/$/ \\/' user
touch  usergrp
chmod +x usergrp

###comando final seleccion grupo##
echo '#!/bin/bash -x' >> usergrp
echo 'user=$(dialog --title "Selec User" --stdout --menu "Opciones" 0 0 0 \' >> usergrp
cat user >> usergrp
echo 'no\ selection "" )' >> usergrp

echo 'samba-tool group addmembers "administrators" $user' >> usergrp
echo 'samba-tool group addmembers "Domain Admins" $user' >> usergrp
echo 'samba-tool group addmembers "enterprise admins" $user' >> usergrp
echo 'samba-tool group addmembers "group policy creator owners" $user' >> usergrp
echo 'sleep 7' >> usergrp

echo 'smbcontrol all reload-config' >> usergrp



echo 'respuesta=$(samba-tool user getgroups "${user}")' >> usergrp

echo '(dialog --title "Groups to which the user $user belongs" --stdout --msgbox "$respuesta" 0 0 )' >> usergrp
./usergrp
cd ..
rm -r userlistadm/
dc-tools



samba-tool group addmembers "administrators" $name
samba-tool group addmembers "Domain Admins" $name
samba-tool group addmembers "Domain Admins" $name
samba-tool group addmembers "enterprise admins" $name
samba-tool group addmembers "group policy creator owners" $name
smbcontrol all reload-config


dc-tools


;;

	18)
##################Edit smb.conf#############################

	        (dialog --title "Read carefully, important" \
        --stdout \
        --msgbox "\nYou are going to edit the domain configuration file,\nautomatically a backup file called smb.conf.save.(date) has been created in /etc/samba/backups.\nIf you spoil the configuration, use any of the files to restore.\n\n  *May the Force be with you*" 0 0 )
	vardate=$(date +"%Y-%m-%d-%T")
	mkdir /etc/samba/backups
	cp /etc/samba/smb.conf /etc/samba/backups/smb.conf.save.$vardate
	cat /etc/samba/smb.conf > /etc/samba/smb.conf.bak
	ls /etc/samba
	sleep 2
	ls /etc/samba/backups
	sleep 3
	dialog --title "smb.conf edited" --editbox /etc/samba/smb.conf.bak 0 0 2> /etc/samba/smb.conf
	returncode=$?
	testparm
	smbcontrol all reload-config
	dc-tools

;;


 	19)
#######################Shutdown###############################
(dialog --infobox "El equipo se apagara en diez segundos" 0 0)
	sleep 1
(dialog --infobox "nueve" 0 0)
        sleep 1
(dialog --infobox "ocho" 0 0)
        sleep 1
(dialog --infobox "siete" 0 0)
        sleep 1
(dialog --infobox "seis" 0 0)
        sleep 1
(dialog --infobox "cinco" 0 0)
        sleep 1
(dialog --infobox "cuatro" 0 0)
        sleep 1
(dialog --infobox "tres" 0 0)
        sleep 1
(dialog --infobox "dos" 0 0)
        sleep 1
(dialog --infobox "uno" 0 0)
        sleep 1


halt -p
;;

	20)
#########################Restart#######################

(dialog --infobox "The machine going to restarted" 0 0)
        sleep 1
(dialog --infobox "nueve" 0 0)
        sleep 1
(dialog --infobox "ocho" 0 0)
        sleep 1
(dialog --infobox "siete" 0 0)
        sleep 1
(dialog --infobox "seis" 0 0)
        sleep 1
(dialog --infobox "cinco" 0 0)
        sleep 1
(dialog --infobox "cuatro" 0 0)
        sleep 1
(dialog --infobox "tres" 0 0)
        sleep 1
(dialog --infobox "dos" 0 0)
        sleep 1
(dialog --infobox "uno" 0 0)

reboot
;;

	21)
##################Exit#############################

	(dialog --title "Exit of application" \
	--stdout \
	--msgbox "Do you want to exit now?" 0 0 )


exit
;;
esac
