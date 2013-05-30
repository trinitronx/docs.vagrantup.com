---
sidebar_current: "provisioning-chefsolo"
---

# Chef Solo Provisioner

**Provisioner name: `:chef_solo`**

The chef solo provisioner allows you to provision the guest using
[Chef](http://www.opscode.com/chef/), specifically with
[Chef Solo](http://docs.opscode.com/chef_solo.html).

Chef solo is ideal for people who are already experienced with Chef,
already have Chef cookbooks, or are looking to learn Chef. Specifically,
this documentation page will not go into how to use Chef or how to write
Chef cookbooks, since Chef is a complete system that is beyond the scope
of a single page of documentation.

<div class="alert alert-warn">
	<p>
		<strong>Warning:</strong> If you're not familiar with Chef and Vagrant already,
		I recommend starting with the <a href="/v2/provisioning/shell.html">shell
		provisioner</a>. However, if you're comfortable with Vagrant already, Vagrant
		is the best way to learn Chef.
	</p>
</div>


## Specifying a Run List

The easiest way to get started with the Chef Solo provisioner is to just
specify a [run list](http://docs.opscode.com/essentials_cookbook_recipes_run_lists.html). This looks like:

```ruby
Vagrant.configure("2") do |config|
  config.vm.provision :chef_solo do |chef|
    chef.add_recipe "apache"
  end
end
```

This causes Vagrant to run Chef Solo with the "apache" cookbook. The cookbooks
by default are looked for in the "cookbooks" directory relative to your
project root. The directory structure ends up looking like this:

```
$ tree
.
|-- Vagrantfile
|-- cookbooks
|   |-- apache
|       |-- recipes
|           |-- default.rb
```

## Custom Cookbooks Path

Instead of using the default "cookbooks" directory, a custom cookbooks
path can also be set via the `cookbooks_path` configuration directive:

```ruby
Vagrant.configure("2") do |config|
  config.vm.provision :chef_solo do |chef|
    chef.cookbooks_path = "my_cookbooks"
  end
end
```

The path can be relative or absolute. If it is relative, it is relative
to the project root.

The configuration value can also be an array of paths:

```ruby
Vagrant.configure("2") do |config|
  config.vm.provision :chef_solo do |chef|
    chef.cookbooks_path = ["cookbooks", "my_cookbooks"]
  end
end
```

## Roles

Vagrant also supports provisioning with [Chef roles](http://docs.opscode.com/essentials_roles.html).
This is done by specifying a path to a roles folder where roles are defined
and by adding roles to your run list:

```ruby
Vagrant.configure("2") do |config|
  config.vm.provision :chef_solo do |chef|
    chef.roles_path = "roles"
    chef.add_role("web")
  end
end
```

Just like the cookbooks path, the roles path is relative to the project
root if a relative path is given.

## Data Bags

[Data bags](http://docs.opscode.com/essentials_data_bags.html) are also
supported by the Chef Solo provisioner. This is done by specifying
a path to your data bags directory:

```ruby
Vagrant.configure("2") do |config|
  config.vm.provision :chef_solo do |chef|
    chef.data_bags_path = "data_bags"
  end
end
```

## Encrypted Data Bags

[Encrypted data bags](http://docs.opscode.com/essentials_data_bags_encrypt.html) are 
now supported by Vagrant as of [this pull request](https://github.com/mitchellh/vagrant/pull/398).
Vagrant will copy the encrypted data bag shared secret key file inside the VM and
will configure the chef provisioner to use it.
To specify the path to your local shared secret file:

```ruby
Vagrant.configure("2") do |config|
  config.vm.provision :chef_solo do |chef|
    chef.data_bags_path = "data_bags"
    chef.encrypted_data_bag_secret_key_path = "#{ENV['HOME']}/.chef/encrypted_data_bag_secret"
  end
end
````

## Custom JSON Data

Additional configuration data for Chef attributes can be passed in
to Chef solo. This is done by setting the `json` property with a Ruby
hash (dictionary-like object), which is converted to JSON and passed
in to Chef:

```
Vagrant.configure("2") do |config|
  config.vm.provision :chef_solo do |chef|
    # ...

    chef.json = {
      "apache" => {
        "listen_address" => "0.0.0.0"
      }
    }
  end
end
```

Hashes, arrays, etc. can be used with the JSON configuration object. Basically,
anything that can be turned cleanly into JSON works.

## Log Level & Debugging

Vagrant now supports setting the output log level of the chef Provisioner
since the resolution of [this issue](https://github.com/mitchellh/vagrant/issues/28).
To set the log level in your Vagrantfile:

```ruby
Vagrant.configure("2") do |config|
  config.vm.provision :chef_solo do |chef|
    # Set chef provisioner log level to one of:
    # [ :debug, :info, :warn, :error, :fatal ]
    # (default: :info)
    chef.log_level = :debug
  end
end
```
