# AWS-CLI EC2 Apache Web Server
[Quelle] (https://towardsaws.com/how-to-use-aws-cli-to-launch-an-ec2-web-server-with-apache-9c20d07e07be)

### Cloud 9 mit GitHub per SSH verbinden/verknüpfen.

### Projekt erstellen

#### Projektverzeichniss erstellen

    mkdir AWS_CLI
    
#### Textdatei für Ressourcen erstellen

    touch ressources.txt
    
#### Überprüfen AMI

    aws ec2 describe-images --owners amazon --filters "Name=name, Values=amzn2-ami-hvm-2.0.????????.?-x86_64-gp2" "Name=state, Values=available" --query "reverse(sort_by(Images, &Name))[:1].ImageId" --output text
    
#### Erstellen Key Pair fürs Projekt

    aws ec2 create-key-pair --key-name TobiasApache --query 'KeyMaterial' --output text > TobiasApache.pem
    
#### Überprüfen

    ls -lah
    
#### Beschreibung von KeyPair

    aws ec2 describe-key-pairs --key-name TobiasApache
    
#### Security Group erstellen

    aws ec2 create-security-group --group-name TobiasApacheSG --description "Allows SSH, HTTP and HTTPS connections for the Web Server"
    
#### Security Group ID:

    sg-0497fe47b9fe1b9ab
    
#### Add rules to Security Group

Now add rule to allow network traffic on TCP port 22 for SSH connections and on TCP port 80 for HTTP connections for your means of connecting to your EC2 instance.

##### Port 22

    aws ec2 authorize-security-group-ingress --group-id sg-0497fe47b9fe1b9ab --protocol tcp --port 22 --cidr 0.0.0.0/0
    
##### Port 80

    aws ec2 authorize-security-group-ingress --group-id sg-0497fe47b9fe1b9ab --protocol tcp --port 80 --cidr 0.0.0.0/0
    
##### Port 443

    aws ec2 authorize-security-group-ingress --group-id sg-0497fe47b9fe1b9ab --protocol tcp --port 443 --cidr 0.0.0.0/0
    
#### Überprüfen sie nun die für die Sicherheitsgruppe erstellten Regeln

    aws ec2 describe-security-groups --group-ids sg-0497fe47b9fe1b9ab
    
#### Erstellen Bash Script Datei (webscript.sh)

    touch webscript.sh
    
#### Erstellen sie zuletzt eine EC2 Instanz mit folgenden Befehl

    aws ec2 run-instances --image-id ami-0b920b0594b5288fb --count 1 --instance-type t2.micro --key-name TobiasApache --security-group-ids sg-0497fe47b9fe1b9ab --user-data file://webscript.sh
    
#### Überprüfen sie ihre installierte EC2 Instaz und den Apache Webserver

    aws ec2 describe-instances
    
#### File Permission

    chmod 400 TobiasApache.pem

#### Verbindung per SSH mit EC2 Instanz (nach jeden Restart neue IP)

    ssh -i TobiasApache.pem ec2-user@3.70.184.142
