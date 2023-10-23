https://www.postgresql.org/docs/current/upgrading.html

# 19.6. Atualizando um cluster PostgreSQL #

Esta seção discute como atualizar os dados do seu banco de dados de uma versão do PostgreSQL para uma mais recente.

Os números de versão atuais do PostgreSQL consistem em um número de versão principal (major) e um número de versão secundário (minor). Por exemplo, no número da versão 10.1, 10 é o número da versão principal e 1 é o número da versão secundária, o que significa que esta seria a primeira versão secundária da versão principal 10. 

Versões secundárias nunca alteram o formato de armazenamento interno e são sempre compatíveis com versões secundárias anteriores e posteriores do mesmo número de versão principal. Por exemplo, a versão 10.1 é compatível com a versão 10.0 e a versão 10.6. Da mesma forma, por exemplo, 9.5.3 é compatível com 9.5.0, 9.5.1 e 9.5.6. Para atualizar entre versões compatíveis, basta substituir os executáveis ​​enquanto o servidor estiver inativo e reiniciar o servidor. O diretório de dados permanece inalterado – pequenas atualizações são simples assim.

Para versões principais do PostgreSQL, o formato interno de armazenamento de dados está sujeito a alterações, complicando assim as atualizações. O método tradicional para mover dados para uma nova versão principal é despejar e restaurar o banco de dados, embora isso possa ser lento. . Um método mais rápido é pg_upgrade. Métodos de replicação também estão disponíveis, conforme discutido abaixo. (Se você estiver usando uma versão pré-empacotada do PostgreSQL, ela poderá fornecer scripts para ajudar nas atualizações de versões principais. Consulte a documentação em nível de pacote para obter detalhes.)


Novas versões principais também normalmente apresentam algumas incompatibilidades visíveis ao usuário, portanto, podem ser necessárias alterações na programação do aplicativo. Todas as alterações visíveis ao usuário estão listadas nas notas de versão (Apêndice E). preste especial atenção à seção intitulada "Migração".


Usuários cautelosos desejarão testar seus aplicativos clientes na nova versão antes de migrar totalmente; portanto, geralmente é uma boa ideia configurar instalações simultâneas de versões antigas e novas. Ao testar uma atualização principal do PostgreSQL, considere as seguintes categorias de possíveis alterações:



# pg_upgrade

https://www.postgresql.org/docs/current/pgupgrade.html

pg_upgrade (anteriormente chamado de pg_migrator) permite que os dados armazenados em arquivos de dados do PostgreSQL sejam atualizados para uma versão principal posterior do PostgreSQL sem o despejo/restauração de dados normalmente necessário para atualizações de versões principais, por exemplo, 

As principais versões do PostgreSQL adicionam regularmente novos recursos que frequentemente alteram o layout das tabelas do sistema, mas o formato interno de armazenamento de dados raramente muda. O pg_upgrade usa esse fato para realizar atualizações rápidas, criando novas tabelas de sistema e simplesmente reutilizando os arquivos antigos de dados do usuário. Se uma versão principal futura alterar o formato de armazenamento de dados de uma forma que torne o formato de dados antigo ilegível, o pg_upgrade não poderá ser usado para tais atualizações. (A comunidade tentará evitar tais situações.)

pg_upgrade faz o possível para garantir que os clusters antigos e novos sejam compatíveis com binários, por exemplo, verificando configurações de tempo de compilação compatíveis,  incluindo binários de 32/64 bits. É importante que quaisquer módulos externos também sejam compatíveis com binários, embora isso não possa ser verificado pelo pg_upgrade.


pg_upgrade oferece suporte a atualizações da versão 9.2.X e posteriores para a versão principal atual do PostgreSQL, incluindo snapshots e versões beta.



