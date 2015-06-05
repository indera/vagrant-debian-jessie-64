# vagrant-debian-jessie-64

This bix was created to experiment with jessie release.

## Steps

* Download jessie iso
```shell
$ wget http://cdimage.debian.org/debian-cd/8.0.0/amd64/iso-cd/debian-8.0.0-amd64-netinst.iso
$ sha1sum debian-8.0.0-amd64-netinst.iso
==>  eab72f4ba73adea580aeaec91649e846aac89fa9  debian-8.0.0-amd64-netinst.iso
```

* Clone the magic repo that lnows how to create *.box files
```shell
$ git clone git@github.com:dotzero/vagrant-debian-jessie-64.git
$ mkdir vagrant-debian-jessie-64/iso
$ mv ../debian-8.0.0-amd64-netinst.iso iso/
$ brew install cdrtools
$ brew install p7zip
$ ./build.sh
...wait ~10 min
==> debian-jessie: Exporting VM...
==> debian-jessie: Compressing package to: /Users/asura/jessie/vagrant-debian-jessie-64/debian-jessie.box
```

* Verify what we got
```shell
$ du -hcs debian-jessie.box
==> 516M    debian-jessie.box
$ sha1sum debian-jessie.box
==> 50df5b2cdfc31bbbcb356f1cd7f2d041fdd17589  debian-jessie.box
```

* Add it to the list of available images
```shell
$ vagrant box add "debian-jessie" debian-jessie.box
```

* Use it in a Vagrantfile
```ruby
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  # config.vm.box = "puphpet/debian75-x64"
  config.vm.box = "debian-jessie"
  config.vm.hostname = "redi-dropper-client"
  config.vm.synced_folder "../app", "/var/www/app"
  config.vm.network "forwarded_port", guest: 80, host: 8088
  config.vm.network :forwarded_port, guest: 443, host: 7088

  config.vm.provision "shell" do |s|
    s.path = "bootstrap.sh"
  end
end
```


# Hosting

GitHub does not allow to store the produced file because the size is more than 100MB

https://help.github.com/articles/configuring-git-large-file-storage/
