# Open Services Gateway Initiative (OSGi)

# Definição
Conjunto de especificações  
Descreve como as aplicações devem definir suas APIs, dependências e serviços.  
Aplicações e infraestrutura modulares  
Facilita os processos de desenvolvimento e testes  
Aumenta o reuso de código  
Acompanhamento em tempo real dos recursos sendo executados pela aplicação  

# Bundle
São os menores "pedaços" do software.  
Pode ser chamado de plugin ao invés de bundle.  
é um agrupamento de funções logicamente relacionadas.  
Pode depender de outros bundles.  
Devem ser dinamicamente instalados, ativados, reativados, atualizados e desinstalados, sem que a JVM seja derrubada.  
Através do `BundleContext` podemos acessar informações do bundle.  

## Manifest

É um arquivo da especificação Java que a OSGi aproveita para definir informações complementares (metadata) do bundle. Seu diretório dentro do plugin é sempre o  `META-INF/MANIFEST.MF`.  

A tabela a seguir mostra os atributos suportados pela OSGi neste arquivo.  

| Nome | Função |
|----------|----------|
| Bundle-Name | Identificar o plugin com um nome mais amigável. |
| Bundle-SymbolicName | Identificar o plugin com um _fully qualified name_ único. Utiliza-se aqui o nome de domínio do autor invertido. |
| Bundle-Version | Conferir ao plugin um número para sua versão. Essa informação deve ser alterada a cada _release_ do plugin.¹ |
| Bundle-Activator | Definir o `BundleActivator` que deve ser utilizado na inicialização e parada do plugin. O uso deste recurso permite que estes dois processos possam ser personalizados (caso necessário). |
| Bundle-ActivationPolicy | Definir se o plugin pode ser inicializado somente caso ele seja realmente necessário. |
| Bundle-ClassPath | Indicar o endereço das classes do bundle. |
| Export-Package | Especifica a API do plugin, ou seja, quais recursos serão expostos para outros plugins consumirem. Pode-se limitar o acesso ao package através do atributo `x-friend="com.my.Bundle`.  |
| Require-Bundle | Lista os plugins que o bundle precisa para sua operação. Com o atributo `;bundle-version="x.x.x"`, é possível escolher a versão da dependência. |
| Import-Package | Determina as APIs dos plugins informados no `Require-Bundle` que serão consumidas pelo bundle. Somente os packages exportados dos outros plugins podem ser utilizados. |

Vale ressaltar que os plugins são identificados unicamente através do conjunto `Bundle-SymbolicName` e `Bundle-Version`.  

> ¹ Na especificação OSGi, é recomendada que as versões sejam criadas no seguinte padrão:
> 
> `<major>.<minor>.<micro>.<qualifier>`
> 
> Onde (extraído de [OSGi Semantic Versioning - Semantic Versions](https://docs.osgi.org/whitepaper/semantic-versioning/040-semantic-versions.html)):
> 
> | Placeholder | Função |
> |----------|----------|
> | major | Packages with versions that have different major parts are not compatible both for providers as well as consumers. For example, 1.2 and 2.3 are completely incompatible. |
> | minor | API consumers are compatible with exporters that have the same major number and an equal or higher minor version. API providers are compatible with exporters that have the same major and minor version number. For example, 1.2 is backward compatible with 1.1 for consumers but for providers it is incompatible. Consumers should therefore import [1.2,2) and providers should import [1.2,1.3) |
> | micro | A difference in the micro part does not signal any backward compatibility issues. The micro number is used to fix bugs that do not affect either consumers or providers of the API. |
> | qualifier | he qualifier is usually used to indicate a build identity, for example a time stamp. Different qualifiers do not signal any backward compatibility issues. |

## Lifecycle
A especificação OSGi prevê alguns estados para os bundles. A tabela a seguir apresenta os estados que um plugin pode apresentar.  

| Estado | Significado |
| ------ | ----------- |
| INSTALLED | O plugin está presente no _container OSGi_, mas suas dependências ainda não foram validadas. |
| RESOLVED | As dependências já foram validadas e preparadas. A partir desse momento, o plugin está pronto para ser iniciado. |
| STARTING | Indica que o plugin está sendo inicializado. |
| ACTIVE | O plugin está sendo executado. |
| STOPPING | O plugin está parado, mas pronto para ser ativado novamente. |
| UNINSTALLED | O plugin está removido do _container OSGi_. |

Quando mais de um plugin pode satisfazer a dependência de um bundle, o que possuir a maior versão será utilizado. Caso hajam mais de um com a mesma versão, o de menor ID será utilizado.  

![Fluxograma](https://www.vogella.com/tutorials/OSGi/img/osgi_livecycle10.png)
Fonte: [Vogella](https://www.vogella.com/tutorials/OSGi/article.html#life-cycle-of-plug-ins).

# Services

É uma forma de estender uma funcionalidade da aplicação com o uso de plugins.  
Podem ser dinamicamente iniciados ou parados.  
Podem ser declarados pela `@Component`.  

## Lifecycle
<TODO>

## Exemplo
<TODO>

# Fontes
- https://www.osgi.org/resources/what-is-osgi/
- https://www.osgi.org/resources/where-to-start/
- https://www.vogella.com/tutorials/OSGi/article.html
- https://docs.osgi.org/whitepaper/semantic-versioning/040-semantic-versions.html
- https://docs.osgi.org/reference/bundle-headers.html
- https://developer.atlassian.com/server/framework/atlassian-sdk/lifecycle-of-a-bundle/
- https://docs.osgi.org/specification/osgi.core/8.0.0/