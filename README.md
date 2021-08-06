## Install Prerequisites
```
pip3 install requests-credssp
pip3 install pywinrm
```

## Configure Windows Host
Download [ConfigureRemotingForAnsible.ps1](https://raw.githubusercontent.com/ansible/ansible/devel/examples/scripts/ConfigureRemotingForAnsible.ps1)
```
powershell.exe -File ConfigureRemotingForAnsible.ps1 -Verbose -EnableCredSSP
```

## Example vars.yml
Example variables to deploy the CertifyTheWeb ACME client to a Windows server:
```
cat << EOF > vars.yml
match_host: servername.domain.com
ansible_connection: winrm
ansible_winrm_transport: credssp
ansible_winrm_server_cert_validation: ignore
package_url: http://192.168.1.1/
ansible_user: "username@domain.com"
ansible_password: "password"
package_name: CertifyTheWebSetup_V5.5.2.exe
install_params: /VERYSILENT /SUPPRESSMSGBOXES /NORESTART /SP-
EOF
```

## Run the playbook
```
ansible-playbook deploy-package.yml -i hosts.yml -e @vars.yml
```