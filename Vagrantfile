Dir[File.expand_path("#{File.dirname(__FILE__)}/plugins/*.rb")].each {|file| require file }
env = UserEnv.load

Vagrant.configure("2") do |config|
  config.vm.box = "generic/ubuntu1604"
  config.vm.network :public_network, bridge: env['switch_name']
  config.vm.synced_folder ".", "/vagrant", type: "smb", smb_username: env['smb_username'], smb_password: env['smb_password']

  config.vm.provider :hyperv do |hv, override|
    hv.vmname = "#{File.basename(Dir.getwd)}-#{Socket.gethostname}"
    hv.memory = 2048
    hv.cpus = 2
    hv.enable_virtualization_extensions = true
    hv.differencing_disk = true
  end

  config.vm.provision 'ansible', run: 'always', type: :ansible_local do |ansible|
    #ansible.galaxy_role_file = 'ansible/requirements.yml'
    #ansible.galaxy_roles_path = '/etc/ansible/roles'
    #ansible.galaxy_command = 'ansible-galaxy install --role-file=%{role_file} --roles-path=%{roles_path}'
    ansible.playbook = 'ansible/playbook.yml'
  end
end
