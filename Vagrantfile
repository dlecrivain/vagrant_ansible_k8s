Vagrant.configure(2) do |config|

  
      config.vm.box = "bento/ubuntu-21.04"
      config.vm.box_url = "bento/ubuntu-21.04"
      config.vm.box_check_update = false
      
# set servers list and their parameters
      NODES = [
        { :hostname => "kub-master-1", :ip => "192.168.12.11", :cpus => 4, :mem => 4096, :type => "kub", :group => "/Kubernetes"},
        { :hostname => "kub-node-1", :ip => "192.168.12.12", :cpus => 2, :mem => 2048, :type => "kub", :group => "/Kubernetes"}, 
        { :hostname => "kub-node-2", :ip => "192.168.12.13", :cpus => 2, :mem => 2048, :type => "kub", :group => "/Kubernetes"},
]

      # run installation
    NODES.each do |node|
      config.vm.define node[:hostname] do |cfg|
              cfg.vm.hostname = node[:hostname]
        cfg.vm.network "private_network", ip: node[:ip]
        cfg.vm.provider "virtualbox" do |v|
          v.customize [ "modifyvm", :id, "--cpus", node[:cpus] ]
          v.customize [ "modifyvm", :id, "--memory", node[:mem] ]
          v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
          v.customize ["modifyvm", :id, "--natdnsproxy1", "on"]
          v.customize ["modifyvm", :id, "--name", node[:hostname] ]
          v.customize ["modifyvm", node[:hostname], "--groups", node[:group] ] 
        end #end provider
              
      end # end config
    end # end nodes
    config.vm.provision "ansible" do |ansible|
    ansible.playbook = "provision.yml"
    ansible.groups = {
      "kubmasters" => ["kub-master-1"],
      "kubnodes" => ["kub-node-[1:2]"],
      #"group3" => ["machine[1:2]"],
      #"group4" => ["other_node-[a:d]"], # silly group definition
      #"all_groups:children" => ["group1", "group2"],
      #"group1:vars" => {"variable1" => 9,
      #                  "variable2" => "example"}
    }
  end #end ansible
end
##Test sous VScode