# Windows Remote Management (WinRM) for Ruby

This is a SOAP library that uses the functionality in Windows Remote
Management(WinRM) to call native object in Windows.  This includes, but is
not limitted to, running batch scripts, powershell scripts and fetching WMI
variables.  For more information on WinRM, please visit Microsoft's WinRM
site: http://msdn.microsoft.com/en-us/library/aa384426(v=VS.85).aspx

## My Info
* Twitter: [@zentourist](https://twitter.com/zentourist)
* BLOG:  [http://distributed-frostbite.blogspot.com/](http://distributed-frostbite.blogspot.com/)
* Add me in LinkedIn:  [http://www.linkedin.com/in/danwanek](http://www.linkedin.com/in/danwanek)
* Find me on irc.freenode.net in #ruby-lang (zenChild)

## Current features

1. GSSAPI support:  This is the default way that Windows authenticates and
   secures WinRM messages. In order for this to work the computer you are
   connecting to must be a part of an Active Directory domain and you must
   have local credentials via kinit. GSSAPI support is dependent on the
   gssapi gem which only supports the MIT Kerberos libraries at this time.

   If you are using this method there is no longer a need to change the
   WinRM service authentication settings. You can simply do a
   'winrm quickconfig' on your server or enable WinRM via group policy and
   everything should be working.

2. Multi-Instance support:  The SOAP back-end has been completely gutted
   and is now using some of the Savon core libraries for parsing and
   building packets. Moving away from Handsoap allows multiple instances
   to be created because the SOAP backend is no longer a Singleton type
   class.


## !!!SHOUTS OUT!!!
Many thanks to the following for their many patches....
* Seth Chisamore (https://github.com/schisamo)
* Paul Morton (https://github.com/pmorton)



## INSTALL:
`gem install -r winrm` then on the server `winrm quickconfig` as admin

## USE:
`require 'winrm'`

## EXAMPLE:
```ruby
require 'winrm'
endpoint = http://mywinrmhost:5985/wsman
krb5_realm = 'EXAMPLE.COM'
winrm = WinRM::WinRMWebService.new(endpoint, :kerberos, :realm => krb5_realm)
winrm.cmd('ipconfig /all') do |stdout, stderr|
  STDOUT.print stdout
  STDERR.print stderr
end
```

There are various connection types you can specify upon initialization:

#### PLAINTEXT
```ruby
WinRM::WinRMWebService.new(endpoint, :plaintext, :user => myuser, :pass => mypass)

# Same but force basic authentication:
WinRM::WinRMWebService.new(endpoint, :plaintext, :user => myuser, :pass => mypass, :basic_auth_only => true)
```

#### SSL
```ruby
WinRM::WinRMWebService.new(endpoint, :ssl, :user => myuser, :pass => mypass)

# Same but force basic authentication:
WinRM::WinRMWebService.new(endpoint, :ssl, :user => myuser, :pass => mypass, :basic_auth_only => true)
```

#### KERBEROS
```ruby
WinRM::WinRMWebService.new(endpoint, :kerberos, :realm => 'MYREALM.COM')
```

## DISCLAIMER
If you see something that could be done better or would like to help out in the development of this code please feel free to clone the repository and send me patches.

`git clone git://github.com/zenchild/WinRM.git` or add an [issue](https://github.com/zenchild/WinRM/issues) on GitHub:

- - -

Cheers!
