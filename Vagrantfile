Vagrant.configure("2") do |db|
  ENV['LC_ALL']='en_US.UTF-8'

  db.vm.provider "virtualbox" do |vb|
     vb.memory = "1024"
     vb.cpus = "1"
     vb.customize ['modifyvm', :id, '--natdnshostresolver1', 'on']
  end

  db.vm.box = "maier/alpine-3.6-x86_64"
  db.vm.network "forwarded_port", guest: 3306, host: 3306
  #db.vm.network "private_network", ip: "10.0.0.11", auto_config: false
  db.vm.synced_folder "./", "/dbinit.d"

  db.vm.provision :shell do |shell|
      #shell.env = {MYSQL_ROOT_PASSWORD:ENV['111111']}
      shell.inline = <<-SHELL
        sudo su
	apk update && apk upgrade
	apk add mysql mysql-client
  	cp /dbinit.d/my.cnf /etc/mysql/my.cnf
	mv /dbinit.d/startup.sh /
	#export MYSQL_ROOT_PASSWORD=${MYSQL_ROOT_PASSWORD}
	#echo ----------
	#echo $MYSQL_ROOT_PASSWORD
	#MYSQL_DATABASE=automosmobile
	#MYSQL_USER=mosu
	#MYSQL_PASSWORD=123
	#MYSQL_ROOT_PASSWORD=111111
	/bin/sh /startup.sh		
     SHELL
  end
end
