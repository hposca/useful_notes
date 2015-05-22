# Dicas para o AWSCLI

- Pega todas as variáveis de ambiente de um ambiente, de uma aplicação:

~~~ Bash
application="safeguard"
environment="safeguard-staging5"
aws elasticbeanstalk describe-configuration-settings --application-name $application --environment-name $environment --query "ConfigurationSettings[].OptionSettings[?OptionName=='EnvironmentVariables']"
~~~

- Pega o nome de uma ou mais instâncias

~~~ Bash
instances="i-12345678 i-09876543"
aws ec2 describe-instances --instance-ids $instances --query "Reservations[].Instances[].Tags[?Key=='Name'].Value" --output=text
~~~
