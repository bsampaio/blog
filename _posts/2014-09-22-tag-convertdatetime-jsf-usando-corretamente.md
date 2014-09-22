---
layout: post
title: "Tag ConvertDateTime JSF - Usando corretamente"
description: "Solução para o problema de conversão errada gerado ao utilizarmos a tag <f:convertDateTime>"
modified: 2014-09-22 09:38:23 -0300
tags: [JAVA, JSF, DEVWEB]
image:
  feature: 
  credit: 
  creditlink: 
comments: 
share: 
---
# Timezone e <f:convertDateTime>

### Tecnologias envolvidas:

1. Java
   1. Spring MVC
   2. JSF

### Descrição do problema

Quando utilizamos JSF, podemos receber um atributo do tipo Calendar e formatá-lo utilizando a tag <f:convertDateTime>. Porém, existe um erro comum que acontece quando deixamos de definir o parâmetro Timezone, que serve para calcular a data corretamente.

### Solução

Utilizando o Spring MVC precisamos enviar um atributo a mais pelo Controller. Esse atributo é nossa Timezone. Caso esse atributo não seja passado, a data apresentada na página pode diferir da data cadastrada, ou seja, temos problema que pode se tornar mais sério ainda de acordo com a importância da data de determinadas informações.

O Java possui uma classe chamada java.util.TimeZone e utilizaremos ela para configurar corretamente a data na página de apresentação.

Segue abaixo um exemplo de código para um Controller:
{% highlight java %}
import java.util.TimeZone;

@Controller
@RequestMapping(/exemplo)
public class ExemploTimeZoneController{
	public String loadPage(ModelMap model){
		/*Obtendo a hora atual*/
		Calendar dataHora = Calendar.getInstance();
		/*
		Comumente adicionariamos a data como atributo do ModelMap
		e retornaríamos a página. Porém, adicionaremos a nossa 
		TimeZone também!*/
		model.addAttribute("dataHora", dataHora);
		//O método getDefault() nos dá a TimeZone local.
		TimeZone timeZone = TimeZone.getDefault();
		model.addAttribute("timeZone", timeZone);

		return "exemplo-datetime";
	}
}
{% endhighlight %}

Logo abaixo temos o exemplo da apresentação da data na página:
{% highlight html %}
<div class="field-box">
        <h:outputLabel value="Exemplo - Data correta"/>
        <h:inputText readonly="true" value="#{dataHora}">
            <f:convertDateTime dateStyle="full" timeZone="#{timeZone}"/>
        </h:inputText>
</div>
{% endhighlight %}

### Conclusão
Seguindo esses passos a sua data aparecerá com os valores corretos.

Para mais detalhes sobre a tag <f:convertDateTime> e seus atributos:

### Links

#### <a target="_blank" href="http://www.tutorialspoint.com">Tutorials Point</a>

<a target="_blank" class="btn primary" href="http://www.tutorialspoint.com/jsf/jsf_convertdatetime_tag.htm"> Mais detalhes... </a>

#### <a target="_blank" href="http://www.jsftoolbox.com/">JSF Toolbox</a>
<a target="_blank" class="btn primary" href="http://www.jsftoolbox.com/documentation/help/12-TagReference/core/f_convertDateTime.html"> Mais detalhes... </a>