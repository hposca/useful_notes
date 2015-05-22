# Dicas para o AWSCLI

Pega todas as variáveis de ambiente de um ambiente, de uma aplicação:

    application="safeguard"
    environment="safeguard-staging5"
    aws elasticbeanstalk describe-configuration-settings --application-name $application --environment-name $environment --query "ConfigurationSettings[].OptionSettings[?OptionName=='EnvironmentVariables']"
