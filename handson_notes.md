# apache kurmak için ====> yum update -y // yum install httpd -y

# github dan dosya kopyalamak için "RAW" halinin linkini kopyalamak gerekir!!!

# **_ USER DATA içerisinde yapılan tüm komutlar ROOT yetkisi ile yapılır. dolayısıyla sudo ve chmod 777 komutuna gerek yoktur. _**

# İndex.html in RAW halinin linkini kopyalayıp son kısmı (index.html) sildik. bu bölüm diğer dosyalar ile aynı.

# yaml dosyası için gerekli komut aşağıdadır...

#! /bin/bash
yum update -y
yum install httpd -y
FOLDER=“https://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@raw.githubusercontent.com/YsnHstrk/kitten-static-website/main"
cd /var/www/html
wget ${FOLDER}/index.html
wget ${FOLDER}/cat0.jpg
wget ${FOLDER}/cat1.jpg
wget ${FOLDER}/cat2.jpg
wget ${FOLDER}/cat3.png
systemctl start httpd
systemctl enable httpd

# sorun çözümü===> githubdan dosya çekemez ise github linkinin önüne token number ekle. "https://tokennumber@xxxxxxxxxx"

# sorun var ise EC2 ya bağlanıp komutları tek tek uygulayarak troubleshooting yapılmalı!!!

# YAML dosyası içerisine "cfn" yazında cfn veya cfn lite seçilir. çıkan menüde:

    -   Description : sitenin neye yaradığına dair açıklama yaz.
    -   Resources :
            + ec2-security groups yazılınca enter! (Kodun ana başlığının üzerinde tıklayınca AWS DOCs kısmına yönlendirir!!!! veya "::" kısmı tamamını kopyala google yapıştır.)
                LogicalID = sec grup adı
                GroupDescription= açıklama (zorunlu)
                SecurityGroupIngress= inbound rules

# Intrinsic Functions- !Sub (substitutes) bu komuttan sonra verilen bazı değerler henüz üretilmemiş olsa bile komutun çalışmasını sağlar.

# image id son sürüm komutu (powershell'de çalışır):

aws ssm get-parameters --names /aws/service/ami-amazon-linux-latest/al2023-ami-kernel-default-x86_64 --query 'Parameters[0].[Value]' --output text

# SecurityGroups ====> Default VPC ile çalışır, genelde VPC ile çalışacağımız için SecurityGroupIds kullanmalıyız....

# Fonksiyon ve alt fonksiyon kullanırken üst fonksiyondan farklı olan kullanımı seç. örnek :

!Base64
"Fn::Sub": string

Fn::Base64:
!Sub string

# Output: Çıktıyı hazır halde gösterir.

# CURL komutu ile yapımı:

#! /bin/bash
yum update -y
yum install httpd -y
FOLDER="https://raw.githubusercontent.com/serdarcw/project-repository/master/Project-101-kittens-carousel-static-website-ec2/static-web/"
curl -s --create-dirs -o "/var/www/html/index.html" -L "$FOLDER"index.html
    curl -s --create-dirs -o "/var/www/html/cat0.jpg" -L "$FOLDER"cat0.jpg
curl -s --create-dirs -o "/var/www/html/cat1.jpg" -L "$FOLDER"cat1.jpg
    curl -s --create-dirs -o "/var/www/html/cat2.jpg" -L "$FOLDER"cat2.jpg
systemctl start httpd
systemctl enable httpd

# LAtest AMI için:

https://docs.aws.amazon.com/linux/al2023/ug/ec2.html#launch-from-cloudformation

Parameters:
LatestAmiId:
Type: 'AWS::SSM::Parameter::Value<AWS::EC2::Image::Id>'
Default: '/aws/service/ami-amazon-linux-latest/al2023-ami-minimal-kernel-default-x86_64'

Resources:
Instance:
Type: 'AWS::EC2::Instance'
Properties:
InstanceType: 't2.large'
ImageId: !Ref LatestAmiId
