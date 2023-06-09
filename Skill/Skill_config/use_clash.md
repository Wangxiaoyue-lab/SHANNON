Clash 是一款基于规则的隧道软件，可以根据配置文件中定义的规则将网络流量路由到不同的代理服务器。

Clash 有三种工作模式:Global、Rule 和 Direct。

在全局模式下，所有网络流量都通过单个代理服务器路由，而不考虑流量的目的地或内容。这种模式类似于传统的“全局代理”，所有流量都通过代理服务器发送。

在规则模式下，网络流量按照配置文件中定义的规则进行路由。这些规则可以根据目的域、IP 地址或协议等因素，为不同类型的流量指定不同的代理服务器。这允许对流量的路由方式进行细粒度控制，对于绕过网络限制或提高网络性能非常有用。

在 Direct 模式下，所有网络流量都直接发送到目的地，而不经过代理服务器。这种模式类似于根本不使用代理 。

全局模式和规则模式之间的主要区别在于对流量路由方式的控制级别。在“全局”模式下，所有的流量都通过一个代理服务器发送。在“规则”模式下，流量可以根据配置文件中定义的规则在不同的代理服务器之间路由。
