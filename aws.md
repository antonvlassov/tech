# Comandos Basicos

Login:

ssh -i /c/Users/supadmin/Desktop/Admin/access-aws-anton.pem ec2-user@ec2-3-16-30-88.us-east-2.compute.amazonaws.com 

Describe:
```
aws <service> <action> [--key value ...]
aws <service> help

aws ec2 describe-instances --filters "Name=instance-type,Values=t2.small"

aws ec2 describe-instances --filters "Name=instance-type,Values=t2.small" --query Reservations[*].Instances[*].KeyName

```

Manutenção em lote das instÇancias Ec2:
```
 UBLICIPADDRESSESS="$(aws ec2 describe-instances --filters "Name=instance-state-name,Values=running" --query "Reservations[].Instances[].PublicIpAddress" --output text)"

for PUBLICIPADDRESS in $PUBLICIPADDRESSESS; do
	  ssh -t "ec2-user@$PUBLICIPADDRESS" "sudo yum -y --security update"
done
```



