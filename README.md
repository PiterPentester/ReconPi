# Recon Pi

ReconPi - Lightweight Recon tool that performs extensive scanning with the latest tools using a Raspberry Pi and Docker.


You can also write output in JSON format as used by Aquatone.

`./subfinder -d freelancer.com -o result_aquatone.json -oA -nW -v `

You can specify custom resolvers too.

`./subfinder -d freelancer.com -o result_aquatone.json -oA -nW -v -r 8.8.8.8,1.1.1.1`
`./subfinder -d freelancer.com -o result_aquatone.json -oA -nW -v -rL resolvers.txt`

``` bash

[x1m@0x01 ReconPi]$ cat recon.sh 
#!/bin/bash

#Check if a URL has been set after recon command.
if [ -d $1 ]; then
    echo 'Please provide a domain: recon example.com'
    exit 1
fi

## VANAF HIERONDER SUBLIST3R VERVANGEN DOOR SUBFINDER DOCKER PI

export URL=$(echo $1)

    #Opens $URL in Sublist3r
    echo 'Starting Sublist3r on' $URL
    if [ "$(expr substr $(uname -s) 1 5)" == "Linux" ]; then 
		python ~/Sublist3r/sublist3r.py -d "$URL" -o domainsfile.txt
	elif [ "$(expr substr $(uname -s) 1 10)" == "MINGW32_NT" ]; then # Probably needs tweaking
		python ~/Sublist3r/sublist3r.py -d "$URL" -o domainsfile.txt
	elif [ "$(expr substr $(uname -s) 1 10)" == "MINGW64_NT" ]; then # Probably needs tweaking
		python ~/Sublist3r/sublist3r.py -d "$URL" -o domainsfile.txt
	elif [ "$(uname)" == "Darwin" ]; then # Probably needs tweaking
		python ~/Sublist3r/sublist3r.py -d "$URL" -o domainsfile.txt
    exit 1
fi

## NIEUWE SUBFINDER COMMAND:

x1m@RPi3:~/subfinder$ docker run -v $HOME/.config/subfinder:/root/.config/subfinder -it subfinder -d yahoo.net > yahoo.net.txt


## Grep domains uit die txt want extra meuk

if [ -e domainsfile.txt ];then
	echo 'Sublist3r Scan complete, checking which domains resolve..'
	while read domain; 
	do if host "$domain" > /dev/null; 
	then echo $domain; 
	fi; 
	done < domainsfile.txt >> resolveddomains.txt
	echo 'Resolved domains written to resolveddomains.txt'
	sleep 1

``` 

Script is working in progress
