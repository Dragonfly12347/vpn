import pexpect

ip_address = '192.168.56.101'
username = 'prne'
password = 'cisco123!'
password_enable = 'class123!'

session = pexpect.spawn ('ssh ' + username + '@' + ip_address, encoding='utf-8', timeout=20)
result = session.expect(['Password:', pexpect.TIMEOUT, pexpect.EOF})

session.sendline(password)
result = session.expect (['>', pexpect.TIMEOUT, pexpect.EOF])

session.sendline('enable')
result = session.expect(['Password:', pexpect.TIMEOUT, pexpect.EOF])

session.sendline(password_enable)
result = session.expect(['#', pexpect.TIMEOUT, pexpect.EOF])

session.sendline('configure terminal')
result = session.expect([r'.\(config\)#', pexpect.TIMEOUT, pexpect.EOF])

session.sendline('hostname Haroon')
result = session.expect([r'Haroon\(config\)#', pexpect.TIMEOUT, pexpect.EOF])

session.sendline('crypto isakmp policy 10')
result = session.expect(['r(config-isakmp\)#', pexpect.TIMEOUT, pexpect.EOF]) 
session.sendline('encryption aes 256')
session.sendline('hash sha256')
session.sendline('authentication pre-share')
session.sendline('lifetime 86400')
session.sendline('exit')

session.sendline('crypto isakmp key mysecurekey address 0.0.0.0')

session.sendline('crypto ipsec transform-set MYTRANSFORM esp-aes 256 esp-sha-hmac')
session.sendline('exit')

session.sendline('access-list 100 permit ip 192.168.56.0 0.0.0.225 192.168.1.0 0.0.0.225')

session.sendline('crypto map MYMAP 10 ipsec-isakmp')
session.sendline('set peer 192.168.1.1')
session.sendline('set transform-set MYTRANSFORM')
session.sendline('match address 100')
session.sendline('exit')

session.sendline('interface GigabitEthernet0/0')
session.sendline('crypto map MYMAP')
session.sendline('exit')

session.sendline('interface Tunnel0')
session.sendline('ip address 10.10.10.1 255.255.255.252')
session.sendline('tunnel source GigabitEthernet0/0')
session.sendline('tunnel destination 192.168.1.1')
session.sendline('tunnel mode ipsec ipv4')
session.sendline('exit')

session.sendline('exit')

print('--------------------------------------------------')
print('')
print('--- Success! connecting to: ', ip_address)
print('---               Username: ', username)
print('---               Password: ', password)
print('')
print('---- VPN has been secured!')
print('--------------------------------------------------')
