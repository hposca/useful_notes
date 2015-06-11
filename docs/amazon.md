# Dicas para o AWSCLI

Conjunto de dicas interessantes para se pegar informações das máquinas que estão na Amazon.

## Pega todas as variáveis de ambiente de um ambiente, de uma aplicação:

~~~ Bash
application="safeguard"
environment="safeguard-staging5"
aws elasticbeanstalk describe-configuration-settings --application-name $application --environment-name $environment --query "ConfigurationSettings[].OptionSettings[?OptionName=='EnvironmentVariables']"
~~~

## Pega o nome de uma ou mais instâncias

~~~ Bash
instances="i-12345678 i-09876543"
aws ec2 describe-instances --instance-ids $instances --query "Reservations[].Instances[].Tags[?Key=='Name'].Value" --output=text
~~~

## Pega informações de qual AMI foi utilizada para se gerar as instâncias:

~~~ Bash
aws ec2 describe-instances --query "Reservations[].Instances[*].[ImageId,Tags[?Key=='Name']]"
~~~