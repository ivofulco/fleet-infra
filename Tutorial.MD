- [FluxCD - GitOps baseado em pull](#fluxcd---gitops-baseado-em-pull)
  - [Vantagens](#vantagens)
- [Funcionamento](#funcionamento)
- [Componentes](#componentes)
  - [Kit de ferramentas GitOps (gotk)](#kit-de-ferramentas-gitops-gotk)
  - [Fontes](#fontes)
  - [Reconciliação](#reconciliação)
  - [Kustomization](#kustomization)
  - [Inicialização](#inicialização)

# FluxCD - GitOps baseado em pull

Flux CD é uma ferramenta declarativa de entrega contínua GitOps para Kubernetes.
O Flux CD segue o padrão GitOps de usar repositórios Git como fonte de verdade para definir o estado desejado do aplicativo.
GitOps é uma maneira de gerenciar sua infraestrutura e aplicativos para que todo o sistema seja descrito declarativamente e controlado por versão (provavelmente em um repositório Git) e tenha um processo automatizado que garante que o ambiente implantado corresponda ao estado especificado em um repositório.

## Vantagens

- Reconciliação contínua: Observe seu repositório Git para ver se há alterações de configuração 24 horas por dia, 7 dias por semana.  
- Painel de controle visual: Para verificar os status e os registros do aplicativo.

# Funcionamento

O funcionamento do Flux é por meio do Kustomize que separa os ambientes em Tenants.

# Componentes

## Kit de ferramentas GitOps (gotk)

No Flux, o GitOps Toolkit se refere a uma coleção de ferramentas especializadas, controladores Flux, APIs componíveis e pacotes Go reutilizáveis ​​disponíveis na organização fluxcd GitHub. Esses componentes são projetados com o propósito de construir fluxos de trabalho de entrega contínua no Kubernetes usando princípios do GitOps. O GitOps Toolkit serve como base para gerenciar e automatizar a implantação e configuração de aplicativos e infraestrutura dentro de um cluster Kubernetes, promovendo uma abordagem declarativa e controlada por versão para operações.

## Fontes

Uma Source define a origem de um repositório contendo o estado desejado do sistema e os requisitos para obtê-lo (por exemplo, credenciais, seletores de versão). Por exemplo, a 1.xtag mais recente disponível de um repositório Git por SSH.

## Reconciliação

Reconciliação refere-se a garantir que um determinado estado (por exemplo, aplicativo em execução no cluster, infraestrutura) corresponda a um estado desejado definido declarativamente em algum lugar.

## Kustomization

O Kustomization recurso personalizado representa um conjunto local de recursos do Kubernetes (por exemplo, kustomize overlay) que o Flux deve reconciliar no cluster. A reconciliação é executada a cada cinco minutos por padrão, mas isso pode ser alterado com .spec.interval. Se você fizer alguma alteração no cluster usando kubectl edit/patch/delete, ela será revertida imediatamente. Você suspende a reconciliação ou envia suas alterações para um repositório Git.

## Inicialização

O processo de instalação dos componentes do Flux de uma maneira GitOps é chamado de bootstrap. Os manifestos são aplicados ao cluster, GitRepositorye Kustomization são criados para os componentes do Flux, então os manifestos são enviados para um repositório Git existente (ou um novo é criado). O Flux pode gerenciar a si mesmo da mesma forma que gerencia outros recursos. O bootstrap é feito usando a fluxCLI ou usando nosso [Terraform Provider.](https://github.com/fluxcd/terraform-provider-flux)