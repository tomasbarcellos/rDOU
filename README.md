
<!-- README.md is generated from README.Rmd. Please edit that file -->
Introdução
==========

Instalação
----------

``` r
# install.packages("devtools")
devtools::install_github("tomasbarcellos/rdou")
```

Exemplo de uso
--------------

### Extração das normas desejadas

``` r
raiz <- system.file("doc", "txt", "DOU1", "2017", "março", "02", package = "rdou")
paginas <- dir(raiz, full.names = TRUE, recursive = TRUE)

library(rdou)
agric <- extrair_normas(paginas, "Agricultura")
faz <- extrair_normas(paginas, "Fazenda")
str(agric, give.attr = FALSE, vec.len = 1)
#> List of 4
#>  $ : chr [1:5] "PORTARIA Nº 27, DE 21 DE FEVEREIRO DE 2017" ...
#>  $ : chr [1:5] "RESOLUÇÃO Nº 2, DE 24 DE FEVEREIRO DE 2017" ...
#>  $ : chr [1:12] "RETIFICAÇÃO" ...
#>  $ : chr [1:7] "PORTARIA Nº 46, DE 21 DE FEVEREIRO DE 2017" ...

str(attributes(faz), vec.len = 1)
#> List of 7
#>  $ class        : chr "norma"
#>  $ orgao        : chr [1:22] "SUPERINTENDÊNCIA DE NORMAS CONTÁBEIS E DE AUDITORIA" ...
#>  $ arquivos     : chr [1:19] "~/R/R-3.5.1/library/rdou/doc/txt/DOU1/2017/março/02/DOU1_2017_03_02_pg001.txt" ...
#>  $ data_dou     : Date[1:1], format: "2017-03-02"
#>  $ secao        : num 1
#>  $ encodificacao: chr "latin1"
#>  $ orgao_alvo   : chr "Ministério da Fazenda"
```

### Tabulação das normas

``` r
df_agric <- estruturar_normas(agric)
dplyr::glimpse(df_agric)
#> Observations: 4
#> Variables: 9
#> $ numero      <int> 27, 2, NA, 46
#> $ tipo        <chr> "PORTARIA", "RESOLUÇÃO", "AVISO DE RETIFICAÇÃO", "...
#> $ orgao       <chr> "SECRETARIA DE DEFESA AGROPECUÁRIA", "SECRETARIA D...
#> $ texto       <chr> "PORTARIA Nº 27, DE 21 DE FEVEREIRO DE 2017\nO Sec...
#> $ promulgacao <date> 2017-03-02, 2017-03-02, 2017-03-02, 2017-03-02
#> $ ementa      <chr> "Credencia a empresa DÍGITOS CERTIFICADORA E IDENT...
#> $ titulo      <chr> "PORTARIA Nº 27, DE 21 DE FEVEREIRO DE 2017", "RES...
#> $ pagina      <int> 5, 5, 6, 6
#> $ secao       <int> 1, 1, 1, 1
```

Usando o pipe
-------------

``` r
download_dou("02/03/2017") %>% 
  converter_pdf(secao = 1) %>% 
  pegar_normas_dou(orgao_alvo = "Agricultura") %>% 
  estruturar_normas()
```
