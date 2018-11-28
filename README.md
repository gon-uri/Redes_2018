### Atención - novedad:
Modificación de las fechas de entrega.

_Miercoles 28/11_ - de 15 a 17
    Clase de consulta para aquellos que deseen discutir cosas del trabajo final

_Viernes 07/12_ - de 15 a 20  (en lugar del miercoles)
   *Presentacion oral* de trabajo final

_Miercoles 12/12_ ULTIMA FECHA para entregar el *informe escrito* del trabajo.


### codigo comunidades con r-igraph
Para aquellos grupos que estaban usando los algoritmos de clustering a traves de **rpy2**: Como charlamos la clase pasada, el orden de las etiquetas que devolvía el codigo **comunidades_con_igraph.ipynb** era desconocido (tenía que ver con el orden que le da a los nodos r-igraph). Por este motivo Agus modificó el codigo de manera que ahora devuelva un diccionario. Esta en el mismo archivo **comunidades_con_igraph.ipynb**. Saludos!

### Guia4

Bibliografia sobre algunos metodos de comunidad:
[Louvain](https://arxiv.org/pdf/0803.0476.pdf)
[Fast greedy](https://arxiv.org/pdf/cond-mat/0408187.pdf)
[Infomap](http://www.mapequation.org/)



Codigo para calcular modularidad utilizando *r-igraph*:
* instalar rpy2: pip3 install --user rpy2
* Si estan en el labs del df ya tienen R i r-igraph instalado. Sino:
** Instalar R
** Instalar igraph

Busquen el codigo en la jupyter notebook de TPC_3 llamada __comunidades_con_igraph__

Codigo para calcular modularidad utilizando *python-igraph*:

```python
import igraph as igraph
import networkx as nx

def clusterize(nx_Graph, method="infomap"):
    """
    Calcula el agrupamiento en comunidades de un grafo.
    
    In:
        nx_Graph: grafo de networkx
        method: metodo de clustering, puede ser: "infomap", "fastgreedy", "eigenvector", "louvain", "edge_betweenness","label_prop", "walktrap", ""
        
    Out:
        labels_dict: diccionario de nodo : a label al cluster al que pertenece.
    """
    if method == "edge_betweenness":
        nx_Graph = max(nx.connected_component_subgraphs(nx_Graph), key=len)#se queda con la componente más grande.
        print("AVISO: restringiendo a la componente connexa más grade. De otro modo falla el algoritmo de detección de comunidades edge_betweenness.")
    
    isdirected = nx.is_directed(nx_Graph)
    np_adj_list = nx.to_numpy_matrix(nx_Graph)
    g = igraph.Graph.Weighted_Adjacency(np_adj_list.tolist(),mode=igraph.ADJ_UPPER)
   
    if method=="infomap":
        labels = g.community_infomap(edge_weights="weight").membership
    if method=="label_prop":
        labels = g.community_label_propagation(weights="weight").membership
    if method=="fastgreedy":
        labels = g.community_fastgreedy(weights="weight").as_clustering().membership
    if method=="eigenvector":
        labels = g.community_leading_eigenvector(weights="weight").membership
    if method=="louvain":
        labels = g.community_multilevel(weights="weight").membership
    if method=="edge_betweenness":
        labels = g.community_edge_betweenness(weights="weight", directed=isdirected).as_clustering().membership
    if method=="walktrap":
        labels = g.community_walktrap(weights="weight").as_clustering().membership
    
    label_dict = {node:label for node,label in zip(nx_Graph.nodes(), labels)}
    return label_dict
```

### TP2:
* **Análisis de vulnerabilidad** Para el gráfico correspondiente a la figura 3 del trabajo de Zotenko, los criterios de remoción de nodos que deben incluir en el trabajo son: Random, Degree, Betweenness y Eigenvalue. Si quieren agregar algún otro criterio, pueden hacerlo, pero no es necesario.
* **Entrega** Recuerden que la fecha límite para entregar el trabajo práctico es el Miercoles 17 de Octubre (antes de la clase). Deberán enviar el TP escrito (y los codigos en uno o varios archivos extra) por correo a ambos docentes de la práctica, con el asunto 'TC2 Redes Complejas - Grupo # - Apellido, Apellido, Apellido'.


![Foto_Universidad_en_Lucha](https://github.com/gon-uri/Redes_2018/blob/master/Fotos/Foto_recortada.jpg)

# Redes_2018
Vamos mantener un repositorio con material que resulte de utilidad para realizar los trabajos prácticos de la materia. Recuerden que además existe una [página web](http://materias.df.uba.ar/redesa2018c2/), donde pueden encontrar cronogramas, guías y otro tipo de material adicional.

### Sobre Git y Github
**Git** es un sistema de control de versiones (VCS) muy utilizado, que permite ordenar y controlar el desarrollo de proyectos (en general proyectos grupales de programación). **Github** es una página que permite almacenar repositorios de **Git** (proyectos) en servidores, y acceder a ellos en forma remota a traves de internet.
Les recomendamos algunos tutoriales de donde pueden aprender como funciona este sistema (hay *mucho* material en la web, pueden usar el que gusten):

* [Porque Git?](https://guides.github.com/introduction/git-handbook/) : Idea básica de Git.
* [Workflow de Git](https://guides.github.com/introduction/flow/) : Explicación del sistema de Branches.
* [Usando Git](http://rogerdudler.github.io/git-guide/index.es.html) : Guía de como crear un repositorio (via linea de comando).
* [Usando Github](https://guides.github.com/activities/hello-world/) : Guía de como crear un repositorio (via página de github).
* [Set Up Git](https://help.github.com/articles/set-up-git/) : Usar github desde la linea de comando.
* [Machete comandos de Git](https://services.github.com/on-demand/downloads/github-git-cheat-sheet.pdf), [Machete 2](https://gist.github.com/davfre/8313299) y [Machete interactivo](http://ndpsoftware.com/git-cheatsheet.html#loc=stash;)


### Sobre Python y R
Durante la cursada, será necesario adquirir conocimientos básicos de **Python** o **R** para poder realizar los trabajos prácticos.  Sabemos que el bagaje de conocimientos en programación dentro del curso es diverso, pero estamos convencidos que todos podrán realizar las prácticas requeridas si le dedican el tiempo suficiente. Les dejamos algunas suguerencias de tutoriales sobre **Python** (que por ceirto es el lenguaje que ambos ayudantes manejamos mejor, y por ende en el que mas los podemos ayudar) para que puedan ir calentando los motores si se sienten fuera de práctica:

* [Python a paso moderado](https://www.programiz.com/python-programming/statement-indentation-comments) : Uno bastante completo. Avanza de a poco, pero avanaza.
* [Python en 10 minutos](https://www.stavros.io/tutorials/python/) : Tutorial rápido, ideal para gente que se muda dede otro lenguaje.
* [Python ejercicios](https://www.practicepython.org/) : Serie de 30 ejercicios que comienzan con lo básico y cubre desde lectura y escritura de archivos hasta scraping básico de html de paginas web. 

### Sobre NetworkX
Es el paquete para trabajar con redes mas usado en **Python**. Existen otros buenos paquetes disponibles, como por ejemplo [igraph](http://igraph.org/python/), pero vamos a usar este ya que es el que está mejor atendido y tiene mayor cantidad de recursos disponibles en la web. Su fuerte no es la vizualización, pero tiene todas las funciones básicas que vamos a precisar para vizualizar. Les dejamos una serie de tutoriales apra que se vayan familiarizando con la libreria:

* [TUTORIAL PRINCIPAL: Python from Luke to Yoda](https://github.com/gon-uri/Redes_2018/blob/master/Python%20from%20Luke%20to%20Yoda.ipynb) : Fue creado espacialemnte para este curso. Es el **tutorial más importante** ya que cubre aspectos básicos tanto de **Python** como de **NetworkX**.
* [Documentación](https://networkx.github.io/documentation/latest/tutorial.html): Es el tutorial de la documentación oficial de la libreria.
