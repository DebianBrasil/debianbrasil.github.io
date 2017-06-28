---
layout: post
title: "Conhecendo o Advanced Packaging Tool (APT)"
date: 2017-06-02 21:31:05
description: "Neste artigo, vamos conhecer um pouco mais o Advanced Packaging Tool (APT), o gerenciador de pacotes do Debian e seus derivados."
main-class: 'dev'
color: '#637a91'
tags:
- APT
- comando
twitter_text: "Favicons, touch icons e tile icons..."
introduction: "Neste artigo, vamos conhecer um pouco mais o Advanced Packaging Tool (APT), o gerenciador de pacotes do Debian e seus derivados."
---
Neste artigo, vamos conhecer um pouco mais o Advanced Packaging Tool (APT), o gerenciador de pacotes do Debian e seus derivados. Então, vamos aprender como esse gerenciador surgiu e alguns comandos que são necessário para os usuários que o utilizam.

No princípio era o .tar.gz. Os usuários tinham que compilar cada programa que quisessem usar em seus sistemas GNU/Linux, ou outro qualquer. Quando o Debian foi criado, uma nova forma de gerenciamento de pacotes tornou-se necessário. Para este sistema, foi dado o nome dpkg. Esse famoso pacote (Pacote é um arquivo binário, compactado ou não, que contém os arquivos binários do software, scripts de configuração, documentação, ícones, imagens, licenças GPL e, por vezes, os próprios arquivos-fonte. Um pacote pode ser criptografado e assinado digitalmente, apesar de nem todos o serem.) foi o primeiro a chegar nos sistemas GNU/Linux. Logo após a Red Hat decidiu criar seu próprio sistema, o RPM.

Um novo dilema rapidamente tomou conta das mentes dos criadores do GNU/Linux. Eles precisavam de um método rápido, prático e eficiente para instalar pacotes, que deveriam gerenciar automaticamente as dependências e cuidar dos arquivos de configuração ao atualizá-los. Aqui novamente, o Debian mostrou o caminho e deu vida ao APT, o ‘Advanced Packaging Tool’ (ferramenta de pacotes avançada), hoje portado pela Conectiva e incorporado por algumas outras distribuições”.

APT é um programa C++ cujo código reside principalmente na biblioteca compartilhada libapt-pkg. Usar uma biblioteca compartilhada para facilitar a criação de interfaces de usuário (front-ends), já que o código contido na biblioteca pode facilmente ser reutilizado. Historicamente, apt-get foi projetado apenas como um front-end de teste para libapt-pkg, mas seu sucesso tende a obscurecer esse fato, tornando-se a ferramenta usada no gerenciamento de pacotes padrão do Debian e seus derivados. Você também pode usar outra ferramentas para gerenciar o APT, como Aptitude, que tem uma interface semi-gráfico e o Synaptic que é uma ferramenta em modo gráfico.
Agora que já possuímos um embasamento sobre a origem e porque foi necessário criar o APT, vamos aprender alguns comandos essenciais.

## Instalação de programas

Para Instalar um programa é muito fácil, basta usar o seguinte comando:

{% highlight  bash %}
sudo apt install nome_do_pacote
{% endhighlight %}

Instalando mais de um programa simultaneamente:

{% highlight  bash %}
sudo apt install pacote1 pacote2 pacote3
{% endhighlight %}


Baixa apenas os pacotes, não são desempacotados e nem instalados. Os arquivos baixados são colocados no diretório /var/cache/apt/archives

{% highlight  bash %}
sudo apt install -d nome_do_pacote
{% endhighlight %}

Simulando a instalação de um pacote, isso não altera nada no seu sistema:

{% highlight  bash %}
sudo apt install -s nome_do_pacote
{% endhighlight %}

>Ainda temos a opção -y que assume “sim” para todas as perguntas, e a opção -u mostrar pacotes que serão atualizados.

## Desinstalação de programas
Para remoção de um programa:

{% highlight  bash %}
sudo apt remove nome_do_pacote
{% endhighlight %}

Removendo mais de um programa simultaneamente:

{% highlight  bash %}
sudo apt remove pacote1 pacote2 pacote3
{% endhighlight %}

Quando você usa “apt remove” para desinstalar um programa, ele ainda deixa suas configurações, por exemplo: Quando você instala o servidor de impressão CUPS e cria uma configuração personalizada. Ao remover com “apt-get remove cups” essas configurações ainda continuam presentes no /etc/, caso você venha a reinstalar esse pacote estas configurações serão mantidas. Para uma desinstalação completa temos que usar esse comando:

{% highlight  bash %}
sudo apt purge nome_do_pacote
{% endhighlight %}

Caso você de alguma forma danifique a instalação de um pacote, ou simplesmente deseja que os arquivos do pacote sejam repostos com a versão mais nova que estiver disponível, você pode usar esse comando:

{% highlight  bash %}
sudo apt --reinstall install nome_do_pacote
{% endhighlight %}


## Atualização
Atualizar lista de repositório do sistema:

{% highlight  bash %}
sudo apt update
{% endhighlight %}

Instalar as atualizações de todos os pacotes no sistema:

{% highlight  bash %}
sudo apt upgrade
{% endhighlight %}

O dist-upgrade é quase igual ao upgrade , o dist-upgrade também atualiza o sistema, mas  pode remover pacotes que ele julgue necessário para atualização do sistema.

{% highlight  bash %}
sudo apt dist-upgrade
{% endhighlight %}

## Obtendo informações sobre os pacotes
Para isso, vamos usar o `apt cahe`. Ela é usada para manipular e obter informações sobre os pacotes no cache (O cache é um sistema de armazenamento temporário usado para acelerar o acesso frequente de dados) do APT.

Esse comando pesquisa por pacotes que estejam relacionados com a palavra-chave que você deseja, e traz uma breve descrição sobre eles.

{% highlight  bash %}
apt search palavra-chave
{% endhighlight %}

Este comando fornece a descrição do pacote, suas dependências, o nome de seu mantenedor, entre outras coisas. Ele traz mais informações do que o apt search.

{% highlight  bash %}
apt show nome_do_pacote
{% endhighlight %}

Mostra as dependência de um pacote:

{% highlight  bash %}
apt depends nome_do_pacote
{% endhighlight %}

Este comando escreve o nome de cada pacote que o APT conhece:

{% highlight  bash %}
apt-cache pkgnames
{% endhighlight %}

Para exibir as prioridades de pacote, se ele está instalado ou não, a versão e outras coisas:

{% highlight  bash %}
apt policy
{% endhighlight %}

## Limpeza

O APT mantém uma cópia de cada arquivo .deb baixado no diretório /var/cache/apt/archives/ . Caso haja atualizações frequentes, este diretório pode rapidamente ocupar uma grande espaço em disco com várias versões de cada pacote. Para efetuar essa limpeza, vamos usar dois comando. O primeiro  esvazia completamente o diretório:

{% highlight  bash %}
sudo apt clean
{% endhighlight %}

O segundo remove apenas os pacotes que não podem mais ser baixados, por terem sumido dos repositórios do Debian, e são agora claramente inúteis:

{% highlight  bash %}
sudo apt autoclean
{% endhighlight %}

O comando a seguir é usado para remover pacotes que foram instalados automaticamente, para satisfazer dependências de outros pacotes, que já não são necessários.

{% highlight  bash %}
sudo apt autoremove
{% endhighlight %}



## Resolvendo Problemas
Às vezes, um defeito em algum pacote ou um download corrompido pode fazer com que o APT fique “travado”, sem concluir a instalação de um determinado pacote por causa de um erro qualquer e sem aceitar instalar outros antes que o problema inicial seja resolvido.
Caso isso aconteça, usamos o sistema de resolução de problemas do APT,  que verifica a lista de dependências quebradas e tenta corrigi-las, instalando pacotes necessários. Para isso, usamos esse comando:

{% highlight  bash %}
sudo apt -f install
{% endhighlight %}

Caso você queira saber qual é a dependência que está faltando, o comando a seguir faz a verificação e mostra todas as dependências que o pacote necessita.

{% highlight  bash %}
sudo dpkg –configure -a
{% endhighlight %}

Caso `apt -f install` não resolva, o que é bem difícil isso acontecer, experimente o `sudo apt -f remove`, que tem uma função similar à do `sudo apt-get -f install`, mas dá preferência a remover os pacotes com problemas, ao invés de tentar corrigir a instalação.
