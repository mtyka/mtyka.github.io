---
layout: post
title:  "Beginner to advanced SSH"
date:   2012-08-22 00:03:44
categories: code 
---

SSH is a wonderful and powerful tool. Most people treat ssh like little more then a remote shell and/or secure file transfer tool (aka scp or rsync).

The basic command line runs like this:

{% highlight bash %}
ssh  username@remotehost
{% endhighlight %}
If you username is the same on the remote host as it is on the curent host you can leave off the username@ and just type ssh remotehost. 
remotehost can bei either a DNS name like mymachine.com or an IP address:

{% highlight bash %}
ssh jeff@134.124.2.12
{% endhighlight %}
Usually this will prompt for a password and then execute a remote shell like bash or so.

Here I’ll discuss some less well known but incredibly powerful features of SSH which make working with remote machines all over the world a dream. I’ll assume you’ve at least use SSH in its simplest form before.

To get an overview of all the options ssh provides (many of which we’ll discuss below), type:

{% highlight bash %}
man ssh
{% endhighlight %}
It will give you an in-depth insight into all the options. Some of the more commonly used ones we’ll discuss below. A full explainaition of the wonders of SSH is well beyond the scope of this article. Instead I will describe the most common use cases and leave it to the reader  to look up the options used and other options of ssh if they are interested.

Forwarding X-windows:

If you use graphical software such as GNUplot ssh enables you to forward the X-windowing graphics through the SSH connection. This mean the graphical window will appear on your local machine. YOu can forward entire window file managers this way if you like and get a true remote desktop experience. To do this use the option -X

{% highlight bash %}
ssh -X username@remotehost
{% endhighlight %}
Port Forwarding (SSH Tunneling):

Port forwarding (also known as SSH Tunneling) is a technique to be able to access a remote machine remotely, even if there’s a machine “in the way” of a direct connection. Imagine the following situation:

localhost <–> firewall <–> workstation

localhost and workstation are on different networks, so that ssh workstation wont work on the laptop because it inhabits a different address space. The naive way around this problem is to do ssh firewall on the laptop, which opens a shell on the firewall. Then, in that shell, I type ssh workstation which gets me a shell on the workstation. Obviously I have to do both steps each time for every shell i want. Then when i need to retrieve a file I again need to issue two scp or rsync commands. This then leaves temporary files filling up the firewall, etc, etc – stupid way of doing it, at least when you’re using that connection frequently.

{% highlight bash %}
ssh -Nf  user@firewall -L 2000:workstation:22
ssh -p 2000 localhost
{% endhighlight %}
The first command sets up the forwarding. It logs into firewall with username user
and creates a forwarding pipe on localhost (your local machine) on port 2000 and forwards all requests to mymachine on port 22 (the ssh port on mymachine). The -N and -f options cause ssh to not actually start a shell on firewall. It only starts the forwarding system. You can leave them out but when you type ‘exit’ on that shell your forwarding port also collapses. Instead -Nf will but it into the background and return to the local shell. The `-L` option sets of the forwarding. It takes 3 subparameters separated by colons in this way: `-L  localport:remotehost:remoteport`. SSH will create a forwarding port on the localmachine on port localport that is forwarded to remotehost on remoteport. In other words, after the command is issued, port 2000 on localhost now behaves as if it was port 22 on mymachine. Thus we can use the second command to ssh into mymachine, by ssh-ing into port 2000 on localhost. If you have a password on mymachine, the seond command will ask you for it. It will behave in every way as if you’re directly interacting with mymachine. All ssh options work too, such as `-X`.

Other SSH tools work too, for example to retrieve a file from remotehost: (for some reason the port option is small -p with ssh and capital -P with scp).

{% highlight bash %}
scp -P 2000 localhost:myremotefile  ./
{% endhighlight %}
Reverse port forwarding or (better) reverse SSH Tunneling

Less well known is reverse forwarding (which sounds stupid i know) better known as reverse SSH Tunneling. The idea is basically that the forwarding port is initiated from the remote machine.

To stick with the above example, assuming ‘laptop’ has an accesible IP address as seen from workstation:

{% highlight bash %}
ssh -R 2000:localhost:22 laptopuser@laptop:
{% endhighlight %}
Now you can SSH from laptop right through to workstation:

{% highlight bash %}
ssh localhost -p 2000
{% endhighlight %}
In other words – if you can get from computer A to computer B using ssh, you can *always* set up a reverse tunnel initiated at A which will allow you to ssh from B to A. This is create to punch holes into firewalls that dont give you outside access. Of course it requires you to initiate the tunnel from A, so, at least occasionally, you need physical access to A. There are ways to automate tunnel creating at boot time though (see later). Note that this can be a) dangerous security wise (it sort of defeats the point of the firewall) and b) may be against the policies of the network/firewall operator. For example I’d advise against doing this to make holes in your cooporate network – that’s bound to get you into trouble. And yes they can detect this ;) . Still, this is occasionally a very useful technique.

Forwarding Web Traffic:

You can also create a VPN via SOCKS proxy with SSH. Do:

{% highlight bash %}
ssh -D 9000 remotehost -Nf
{% endhighlight %}
Now you have a local SOCKS proxy running on localhost listening at port 9000. Again -Nf just means ssh will set up the forwarding ports and then return to the local shell.

Now, go to, say, Firefox. Go to Preferences->Advanced->Network->Connection->Settings

Click on manual proxy configuration and type in “localhost” under “SOCKS Host” and set the port to 9000. Click Ok to save the setting. Now if you go to any webpage, what will happen is that all the webrequests will be forwarded via encrypted SSH connection to remotehost and appear to come from remotehost for anyone analyzing your traffic. This is a great way to

a) circumvent a local firewall and/or local censoring of sites, IP addresses or DNS requests.

b) use the remotehosts’ privileges remotely. For example if you work at a university and the university pays subscriptions to scientific journals, you can often see papers behind paywalls from your machine at work. But at home, you dont have those privileges. Solution ? Just ssh -D into your workstation from home and use the SOCKS proxy to redirect your traffic through your workmachine and tada – you can get to everywhere you can get to from work. Same goes for, say, intranet sites that are only visible from within the company or university network.

Now lets imagine your work machine is behind a firewall, so you need two hops to get to it via SSH. Can you still use -D ? Yes! Just combine what we learned above about port forwarding with what we learned just now about SOCKS proxies:

{% highlight bash %}
ssh -Nf user@firewall -L 2000:workstation:22
ssh -Nf -D 9000 -p 2000 user@localhost
{% endhighlight %}
Done! Now you have a local forwarding port on port 2000 and a SOCKS proxy on port 9000. Any web traffic will now go to the Proxy on local port 9000 then via the forwarding port, via the firewall to your workstation and appear to be comming from workstation. Awesome, right ?

Don’t have a remote access via the firewall but you have a static IP at home ? Just use reverse forwarding (explained above) together with a SOCKS proxy. You have to set up the first part while at work, and then the second part at home from your home machine. I’ll leave the details for homework ;)

Using SSH keys:

Type:

{% highlight bash %}
ssh-keygen -f ~/.ssh/mynewkey
{% endhighlight %}
It will prompt you for a password – you can either just hit enter to create a password-less keypair (unencrypted) or type in a password to lock the key with a password. In general its better to password protect the key, but if you’re trying to set up passwordless logins on a local network having no password is ok. This step will generate a 2048 RSA key pair. It consists of two files: `~/.ssh/mynewkey` and `~/.ssh/mynewkey.pub`. The former is the secret private key. You should keep this secure by leaving the file permissions set to 600. Also, you may consider making a backup on a secure medium such as an password encrypted thumbdrive which is stored with your other valuable (safelock etc). If you loose the private key there is no way to regenerate it from the public key and thus you’ll be locked out of all the machines that require the key to log in.

The public key is what needs to go to any machine you want to log in to. It can be sent in plain sight, say, by email. If anyone obtains the public key its ok – it will not help them log into anything. The public key is like a marker – any machine that has the public key can be accessed from any machine with the private key – but not the other way around. That’s why its called asymmetric cryptography – its one-directional.

Ok – enough theory – how do we get this to work ?

On the host open the file `~/.ssh/authorized_keys2` and paste in a new line containing the entire SSH public key:

{% highlight bash %}
ssh-rsa AAAAB3NzaC1yc2EAAAABIwAAAQEAqKzKjn+XfOjT+RCqG6ERmsPJRzzIn0yjCtiRSvp+nkVFrUWhFyIOv0ooEmge+Zw6okwf+RYZIWuYr2F+DFJVw3WMb2wHQ2ULG4RsTHoTAhCaUK1HlODvqNc5X5rymhnnv0VaqKgQIlLid+OnxAu1meyeKYhNTaow1lUdkJslN408k9RXWkt6nmETvvlPTyxGF/GKh1+qIUCxu7UzOH+m/kyJslES+TxK5KipVkawMeS8keTVfFmberV5GPtohVxDn18EdR1newF/03WTOYJmmJLqNwHeo1xlfzkQoSfGeWs3BMirPnuZKtpyE4DFw48LIrN2GCKQM1vwgcLjIHJwZw== user@localhost
{% endhighlight %}
Make sure that the owner of the file is the owner of the account (not “root” or whatever). Also ensure that the filesystem rights to the file are set correctly.

{% highlight bash %}
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys2
{% endhighlight %}
Ok, now on the client system, the private key should already be in ~/.ssh/privatekey again with filesystem rights set to 600

Now you can log into the host using your key pair:

{% highlight bash %}
ssh -i ~/.ssh/privatekey hostname
{% endhighlight %}
(SSH will be default look for ~/.ssh/id_rsa, so if you named your key id_rsa you actually dont need the -i parameter at all )

Now, if you put the authorized keys and the private key in the .ssh directory of both machines (and both machines run an ssh server deamon (sshd)) you should be able to cross login from either machine to the other. If your key was generated *without* password it won’t even promt you for any passwords during login. You just go straight in. This is awesome for remote deploying jobs via ssh on remote machines using scripts:

{% highlight bash %}
ssh -i ~/.ssh/mykey user@remotemachine  remote_executable param1 param2 param3
{% endhighlight %}
You can also add the -f  option which will result in a non-blocking call. So ssh will immediately return to your local prompt but the command will run remotely anyway. Without the -f option ssh will block the prompt until the remote job completes.

Why are keys safer then passwords ?

A password protected keyed login is in many ways safer then a passworded login. Its much harder to bruteforce guess a key then a password, simply because keys are longer then typical passwords and, unlike passwords, dont suffer from being often derivatives of words, phrases etc that effectively lower the entropy in the password (the amount of true randomness). OTOH, as long as the key is protected by a password, even if the source machine gets compromised, the key doesnt automatically let anyone in – you still have to get the password to the key to decrypt it. This is why key-less keys can be dangerous because they rely on the security of the secret key itself.

Using ~/.ssh/config

Often you will type the same ssh commands over and over again. Maybe you always use a particular firewall or server. Maybe you have different usernames on different servers. Many of the repetitive actions can be automised and customised – how to do this is the focus of this section:

The file `~/.ssh/config` sets settings for various hosts. The basic syntax is:

{% highlight bash %}
host hostmask
setting  value
setting2  value2
...
{% endhighlight %}
Ok, lets jump right in. The simplest example is just setting up an alias hostname. Lets put this in our config file

host myhost
Hostname myhost.subnet.longdomainname.ac.uk
Now you can ssh myhost and ssh will automatically connect to the host myhost.subnet.longdomainname.ac.uk. Alternatively you could have specified a raw IP address, that works too. I hear you ask – why not just use /etc/hosts to set up aliases ? yes you can do that too – its a more low level way of doing it. But you need sudo/root access to the machine to do that. If you’re on a machine you don’t have those, then the `~/.ssh/config` way of achieving this still works.

Next, lets imagine the remote user is different. On your local machine your username is Matt. But on your remote server myhost it is m2456. This means everytime you log into the remote server you need to prepend m2456@...  . Instead you can do this:

{% highlight bash %}
host myhost
Hostname myhost.subnet.longdomainname.ac.uk
User m2456
{% endhighlight %}
Now instead of saying ssh m2456@myhost.subnet.longdomainname.ac.uk you just say ssh myhost or scp myfile myhost:. Neat, right ? This can save a lot of typing.

You can specify wildcards too.

{% highlight bash %}
host *
ForwardX11  yes
{% endhighlight %}
will have the same effect as if you added -X to every ssh command you issue, i.e. X11 calls will automatically be forwarded.

Or you can do automatic port forwarding too!

{% highlight bash %}
host myfirewall
Hostname myfirewall.domain.com
LocalForward 127.0.0.1:3000 workstation:22
{% endhighlight %}
will try and create a port forwarding on the local machine on port 3000 to the remote workstation on port 22. (workstation is behind the firewall myfirewall.domain.com)

I maintain different keys for different servers. Having to do ssh particularhost -i ~/.ssh/keytoparticularhost is cumbersome. Instead I just add the Identityfile setting to my config file for each host when I set it up:

{% highlight bash %}
host particularhost
User remoteuser
Identityfile ~/.ssh/keytoparticularhost
{% endhighlight %}
Done. now i just say ssh particularhost and everything else happens automatically.

It doesnt stop here: you can set all sorts of settings for hostnames. Type in man ssh_config to see the full list of options and a description of them.

