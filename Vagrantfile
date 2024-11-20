Vagrant.configure("2") do |config|

  config.vm.define "zeus" do |zeus|
    zeus.vm.box = "debian/bullseye64"
    zeus.vm.network "forwarded_port", guest: 80, host:8080
    config.vm.synced_folder "./web", "/home/vagrant/shared"
    zeus.vm.provision "shell", inline: <<-COMAND
        apt-get update && apt-get upgrade -y
        apt-get install -y nginx 
        apt-get install -y git
        sudo mkdir -p /var/www/web/html
        sudo chown -R www-data:www-data /var/www/web/html
        sudo chmod -R 755 /var/www/web
        cp /home/vagrant/shared/web /etc/nginx/sites-available/web 
        sudo ln -s /etc/nginx/sites-available/web /etc/nginx/sites-enabled/
        systemctl restart nginx
        apt-get install vsftpd
        sudo mkdir -p ftpserver/debian/ftp
        sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/vsftpd.key -out /etc/ssl/certs/vsftpd.crt
        sudo nano /etc/vsftpd.conf
        cp /home/vagrant/shared/vsftpd.conf /etc/vsftpd.conf 
        sudo systemctl restart vsftpd
        dpkg -l | grep openssl
        cp /home/vagrant/shared /etc/nginx/.htpasswd
        cp /home/vagrant/shared/html/* 
    COMAND
  end
end
