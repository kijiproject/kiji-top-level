begin
  ENV['VAGRANT_DEFAULT_PROVIDER'] ||= File.read('.vagrant/provider').strip
rescue Errno::ENOENT
end

# Choose what platform to provision with the PLATFORM environment variable.
ENV['PLATFORM'] ||= 'cdh4.4.0'

# http://docs.vagrantup.com/v2/
Vagrant.configure('2') do |config|
  config.vm.box = 'precise64'
  config.vm.hostname = 'kiji'
  config.ssh.forward_agent = true

  config.vm.network 'forwarded_port', guest: 50070, host: 50070 # Namenode
  config.vm.network 'forwarded_port', guest: 50090, host: 50090 # 2ndary Namenode
  config.vm.network 'forwarded_port', guest: 50075, host: 50075 # Datanode
  config.vm.network 'forwarded_port', guest: 50030, host: 50030 # Job Tracker
  config.vm.network 'forwarded_port', guest: 50060, host: 50060 # Task Tracker
  config.vm.network 'forwarded_port', guest: 60010, host: 60010 # HBase

  config.vm.provider 'vmware_fusion' do |provider, override|
    # provider.gui = true
    override.vm.box_url = 'http://files.vagrantup.com/precise64_vmware.box'
    provider.vmx['memsize'] = '3072'  # VMware refuses to start for anything larger
    provider.vmx['numvcpus'] = '1'    # http://superuser.com/q/505711/96477
  end

  config.vm.provider 'vmware_workstation' do |provider, override|
    # provider.gui = true
    override.vm.box_url = 'http://files.vagrantup.com/precise64_vmware.box'
    provider.vmx['memsize'] = '3072'
    provider.vmx['numvcpus'] = '1'
  end

  config.vm.provider 'virtualbox' do |provider, override|
    # provider.gui = true
    override.vm.box_url = 'http://files.vagrantup.com/precise64.box'
    provider.name = 'kiji'
    provider.customize ['modifyvm', :id, '--memory', '3135'] # shrinks to 3072 inside
    provider.customize ['modifyvm', :id, '--cpus', '2']
  end

  config.vm.provision :shell, privileged: false,
    inline: "export PLATFORM='#{ENV['PLATFORM']}'; cd /vagrant; ./provisioning/vagrant"
end
