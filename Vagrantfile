require 'inifile'
require 'date'
require 'vagrant-aws'
require 'yaml'

Vagrant.configure("2") do |config|
  config.vm.define "winVDI-Test" do |winvdi|
    winvdi.vm.box = "aws-dummy"
    winvdi.vm.synced_folder ".", "/vagrant", disabled: true 
    winvdi.vm.guest = "windows"
    winvdi.vm.boot_timeout = 600
  
    winvdi.vm.provider :aws do |aws, override|
      aws_config = YAML::load_file(File.join(Dir.home,".aws_secrets")) 
      aws.access_key_id = aws_config.fetch("access_key_id")
      aws.secret_access_key = aws.config.fetch("secret_access_key")
      aws.keypair_name = aws_config.fetch("keypair_name") 
      aws.ami = "ami-c343ecb0" # Microsoft Windows Server 2012 Base
      aws.instance_type = "t2.small"
#     aws.instance_type = "t2.micro"
      aws.region = aws_config.fetch("aws_region")
      aws.terminate_on_shutdown = true
      aws.security_groups = aws_config.fetch("security_groups")
      aws.subnet_id = aws_config.fetch("security_groups")
      aws.associate_public_ip = true
  
      # aws.spot_instance = true
      # aws.spot_max_price = 0.0155
      # aws.spot_valid_until = DateTime.now + (3.0/24)
      aws.tags = {
        'Name' => 'winVDI-Test'
      }
  
      aws.user_data = File.read("user_data.txt")
  
      override.vm.communicator = "winrm"
      override.winrm.username = "Administrator"
      override.winrm.password = "VagrantRocks"
  
      override.vm.synced_folder ".", "/vagrant", disabled: true
    end
  end
end
