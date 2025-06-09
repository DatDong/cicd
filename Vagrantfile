Vagrant.configure("2") do |config|
  # Sử dụng Ubuntu box
  config.vm.box = "ubuntu/bionic64"

  # Bật SSH Agent Forwarding để máy ảo clone GitLab bằng SSH key của máy host
  config.ssh.forward_agent = true

  # Mở cổng 80 trong máy ảo → cổng 8080 trên máy host
  config.vm.network "forwarded_port", guest: 80, host: 8080

  # Provision máy ảo
  config.vm.provision "shell", inline: <<-SHELL
    # Cập nhật package
    sudo apt-get update

    # Cài Git để clone GitLab repo
    sudo apt-get install -y git

    # Cài Nginx (web server)
    sudo apt-get install -y nginx

    # Enable Nginx
    sudo systemctl enable nginx
    sudo systemctl start nginx

    # Tạo thư mục project
    mkdir -p /home/vagrant/project

    # Clone GitLab repo (SSH URL)
    if [ ! -d "/home/vagrant/project/.git" ]; then
      git clone git@gitlab.com:DatDong/cicd.git
    else
      echo "Repo already cloned."
    fi

    # (Optional) Copy index.html từ repo vào Nginx (nếu muốn)
    if [ -f "/home/vagrant/project/index.html" ]; then
      sudo cp /home/vagrant/project/index.html /var/www/html/index.html
      sudo systemctl restart nginx
    fi
  SHELL
end
