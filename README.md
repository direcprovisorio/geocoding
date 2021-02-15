# **Geocoding**
*Scripts em R referentes ao paper "Improving geocoding matching rates of structured addresses in Rio de Janeiro".*
 
__________________________________________________________________________________________________________

### **Descrição geral** 
O processo de georreferencimento incluiu as etapas de padronização dos endereços, geocodificação por meio da API do google maps e validação dos resultados.

*Para a execução sequencial dos scripts é necessário:*

- Baixar os arquivos de dados (endereços, dados referência, dicionários etc) no diretório escolhido. 
 Todos os "dicionários" utilizados são referentes ao Rio de Janeiro, mas podem ser adaptados para outros locais e base de dados.
 [Alguns dicionários contem expressoes regulares](https://rstudio.com/wp-content/uploads/2016/09/RegExCheatsheet.pdf).

- Para os bancos com formatos diferentes do SIM, [indicar os nomes das colunas do banco de dados referentes ao campo do endereço (logradouro, bairro etc)](
https://github.com/direcprovisorio/geocoding/blob/2c2bf37a962be42e7e3e2bd70aaa9533048ff0cd/script1_padronizacaobasica.R#L55-L66)


- Para editar a base de dados dos correios - [Diretório Nacional de Endereço - DNE](https://www.correios.com.br/enviar-e-receber/marketing-direto/diretorio-nacional-de-enderecos-dne) - é necessario salvar (em formato csv) os arquivos da pasta 'delimitado' referentes ao local do estudo: em geral, LOG_LOGRADOURO,      LOG_GRANDE USUARIO e LOG BAIRRO). Para facilitar, salve os arquivos com separacao por '@'.

-  Indicar a chave (API key) do projeto. Para utilizar a API do google é necessário [cadastrar o projeto e ativar a API key](https://developers.google.com/maps/documentation/geocoding/start).

```#Inserir a chave do projeto 
gkey = "xxxxxxxxxxxxxxxxxxx"
```
__________________________________________________________________________________________________________

### **Descrição dos scripts**

#### Script 1: **Padronização básica dos endereços** 

- Recuperação das informações de bairro que estão como NA/missing no campo do bairro, mas que aparecem nos campos complemento ou nome do logradouro
- Padronização das informações referentes ao bairro utilizando o dicionário de bairros 
- Recuperacao das informações referentes ao número e ao logradouro por meio do campo complemento
- Padronização do logradouro, correção de erros (mais frequentes) no nome do logradouro, conversão de números por extenso e abreviações, 
- Criação do endereço completo (sem DNE)

#### Script 2: **Edição da base de dados dos correios (DNE)**

- Criação do campo número/intervalo e lado (utilizando o LOG_COMPLEMENTO) e do campo para os nomes alternativos de logradouro e bairro
- Construção das listas de nomes, tipos e pares de tipos de logradouros 
- Padronização básica de todos os campos de endereço do DNE.

#### Script 3: **"Dicionário" automático de logradouros** 

- Criação do dicionário/corretor de logradouro, utilizando regras de erros de grafia aplicadas à base de enderecos dos correios
- Aplicação do dicionário corretor nos endereços do banco original e a criação do endereço completo final 

#### Script 4: **Linkage com a base de dados dos correios (DNE)**

- Recuperação das informações de logradouro e bairro dos endereços com número de cep válido (8 dígitos)
- Linkage com a base DNE (para os endereços sem cep válido), utilizando o logradouro como campo de blocagem.

#### Script 5: **Geocodificação por meio da API do google** 

- Geocodificação (geocode-ggmap)
- Extração dos campos de interesse (status da busca, endereco completo, latitute, longitude, tipo (rooftop, range_interpolated, geometric_center, approximated), matching parcial.

#### Script 6: **Validação dos endereços** 

- Separação e padronização dos campos de endereço retornados pelo google.
- Comparação entre os campos (logradouro, número, bairro, cidade) do endereco original e os campos do endereço retornado pelo google.
- Comparação entre as informações de logradouro e bairro obtidas por meio da base dos correios. 


*Atualizações dos scripts serão publicadas diretamente na aba principal. A versão original estará na pasta (versão_paper2021)*
