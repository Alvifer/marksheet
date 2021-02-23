---
Capa: post
Título: "<strong>anidado</strong> en Sass"
Subtítulo: "Reutilizando el mismo selector principal"
sección: sass
---

### Sintaxis

En Sass, **anidar reglas CSS** permite definir **selectores de jerarquía**:

{% highlight scss %}
.title{
  strong{}
  em{}
}
{% endhighlight %}

This will be compiled into:

{% highlight css %}
.title{}
.title strong{}
.title em{}
{% endhighlight %}

Debido a que `strong` y `em` aparecen _dentro_ de la regla `.title` (entre las 2 llaves` {``} `), ambos se **antepondrán** con el selector principal` .title`.

### Propósito de anidación

Debido a que [CSS priority](/css-priority.html) puede ser complicado, es común usar ser **específico** al escribir selectores, combinando múltiples clases/etiquetas para evitar que las reglas CSS se cancelen entre sí.

{% highlight css %}
.description{}
.description p{}
.description p a{}
.description p a:hover{}
.description p strong{}
.description table{}
.description table tr{}
.description table tr:nth-child(2n){}
.description table th,
.description table td{}
.description table td.empty,
.description table th.empty{}
.description table th{}
{% endhighlight %}

Para evitar la reescritura de `.description`, usemos el signo comercial `&`:

{% highlight scss %}
.description{
  p{}
  p a{}
  p a:hover{}
  p strong{}
  table{}
  table tr{}
  table tr:nth-child(2n){}
  table th,
  table td{}
  table th{}
  table td.empty,
  table th.empty{}
}
{% endhighlight %}

Puede ir aún más lejos reemplazando `& p` y `& table` por `&` para crear selectores **anidados**:

{% highlight scss %}
.description{
  p{
    a{
      &:hover{}
    }
    strong{}
  }
  table{
    tr{
      &:nth-child(2n){}
    }
    th,
    td{
      &.empty{}
    }
    th{}
  }
}
{% endhighlight %}

Remember **[HTML nesting](/html-hierarchy.html)**? The indentation in Sass allows to _replicate_ how HTML elements are nested.

Notice how we only wrote `table` and `.empty` **once** for example.

It will generate exactly the CSS we started with:

{% highlight css %}
.description{}
.description p{}
.description p a{}
.description p a:hover{}
.description p strong{}
.description table{}
.description table tr{}
.description table tr:nth-child(2n){}
.description table th,
.description table td{}
.description table td.empty,
.description table th.empty{}
.description table th{}
{% endhighlight %}

### The ampersand parent selector

When you nest selectors in Sass, it basically adds a **space** between the _parent_ selector and the _child_ one. So:

{% highlight scss %}
//scss
.parent{
  .child{}
}

// becomes in css
.parent .child{}
{% endhighlight %}

The **space** between `.parent` and `.child` defines a **hierarchy**: this selector targets HTML elements with `class="child"` nested _within_ `class="parent"`.

Now, what if you want to use **pseudo-selectors** like `:hover`? Or you want to have a selector with **joined classes**? You can use the **ampersand** which is shortcut for the parent selector:

{% highlight scss %}
//scss
.parent{
  &:hover{}
  &.other-class{}
}

// becomes in css
.parent:hover{}
.parent.other-class{}
{% endhighlight %}

Notice how there is **no space** between `.parent` and either `:hover` or `.other-class`.

The `.parent.other-class` will target HTML elements that have `class="parent other-class"`.

### Full example

{% highlight css %}
.post-content{}
.post-content a{}
.post-content a:hover{}
.post-content aside{}
.post-content blockquote{}
.post-content code{}
.post-content h3{}
.post-content h3 a{}
.post-content h4{}
.post-content h4:before{}
.post-content h4:after{}
.post-content p{}
.post-content p:first-child{}
.post-content p:last-child{}
.post-content ul{}
.post-content ul ul{}
.post-content ul ul ul{}
.post-content dl{}
.post-content dl:before{}
.post-content dl dt{}
.post-content dl dd{}
.post-content pre{}
.post-content pre code{}
.post-content table{}
.post-content table tr{}
.post-content table tr:nth-child(2n){}
.post-content table th,
.post-content table td{}
.post-content table th{}
.post-content table td.empty,
.post-content table th.empty{}
.post-content table code{}
.post-content table pre{}
.post-content table pre:before{}
{% endhighlight %}

{% highlight scss %}
.post-content{
  a{
    &:hover{}
  }
  aside{}
  blockquote{}
  code{}
  h3{
    a{}
  }
  h4{
    &:before{}
    &:after{}
  }
  p{
    &:first-child{}
    &:last-child{}
  }
  ul{
    ul{
      ul{}
    }
  }
  dl{
    &:before{}
    dt{}
    dd{}
  }
  pre{
    code{}
  }
  table{
    tr{
      &:nth-child(2n){}
    }
    th,
    td{
      &.empty{}
    }
    th{}
    code{}
    pre{
      &:before{}
    }
  }
}
{% endhighlight %}

