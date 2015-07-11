---
layout: post
title: Rmarkdown syntax
category: DataScience
tags: [R, Rmarkdown]
---


## Rmarkdown


1.Do not echo code

```
echo=FALSE
```

2.hide results

```
results="hide"
```

3.Inline text computations

> \`\`\`{r computetime}  
>time <- format(Sys.time(), "%a %b %d %X %Y")  
>rand <- rnorm(1)  
> \`\`\`

use time and rand

```
The current time is 'r time'.
My favorite random number is 'r rand'.
```

4.Incorporating Graphics

> \`\`\`{r showtable, results="asis"}  
>library(xtable)  
>xt <- xtable(summary(fit))  
>print(xt, type="html")  
> \`\`\`

5.Set globle options

> \`\`\`{r setoptions, echo=FALSE}  
>opts_chunk$set(echo=FALSE, results="hide")  
> \`\`\`

6.Some common options

```
fig.height: numeric
fig.width: numeric
cache=TRUE
```
