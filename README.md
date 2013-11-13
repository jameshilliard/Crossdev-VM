In order to run this vagrant-vbguest version 0.10.0.pre1 or newer must be installed this is due to the virtualbox guests on the ubuntu cloud images being incompatible
Install Command below:
vagrant plugin install vagrant-vbguest --plugin-version 0.10.0.pre1 --plugin-prerelease --plugin-source https://rubygems.org

Virtualbox versions below svn 90468 are currently broken for private networking, download this test version or something newer:
http://www.virtualbox.org/download/testcase/VirtualBox-4.3.3-90468-Win.exe
https://www.virtualbox.org/ticket/12182#comment:9
https://github.com/mitchellh/vagrant/issues/2392

Vagrant may generate a new host-only adapter if your ip is not in the current range so you may end up with duplicates

Vagrant/virtualbox may kill your host computer internet if this happens just reboot and make sure you adapters are reset. 
This has something to do with host-only internet but I'm not all that sure on the specifics.

