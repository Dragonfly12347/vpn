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
result = session.expect(['Password:', pexpect.TIMEOUT, pexpect.EOF})

session.sendline('password_enable')
result = session.expect(['#', pexpect.TIMEOUT, pexpect.EOF})

session.sendline('configure terminal')
result = session.expect([r'.\(config\)#', pexpect.TIMEOUT, pexpect.EOF})

session.sendline('hostname Haroon')
result = session.expect([r'Haroon\(config\)#', pexpect.TIMEOUT, pexpect.EOF})

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
