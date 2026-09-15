# Node.js + observabilidade e seguranca

## Subir na EC2

Instale Docker e o Docker Compose plugin na instancia. Para o Elasticsearch, configure o limite do kernel antes de iniciar:

```bash
sudo sysctl -w vm.max_map_count=262144
echo "vm.max_map_count=262144" | sudo tee /etc/sysctl.d/99-elasticsearch.conf
docker compose up -d --build
```

Verifique os containers com `docker compose ps` e acompanhe a inicializacao com `docker compose logs -f`.

## Portas

| Servico | Porta |
|---|---:|
| Aplicacao Node.js | 3000 |
| Prometheus | 9090 |
| Grafana | 3001 |
| Elasticsearch | 9200 |
| Filebeat | somente rede Docker |
| Logstash Beats/API | 5044/9600 |
| Kibana | 5601 |
| OWASP ZAP API | 8090 |
| SonarQube | 9000 |

O Grafana usa `admin` / `admin` por padrao. Defina `GRAFANA_ADMIN_PASSWORD` no ambiente antes de subir em producao. Elasticsearch e Kibana estao configurados sem autenticacao para o ambiente inicial; restrinja as portas no Security Group da EC2 e habilite autenticacao antes de expor esses servicos a internet.

No Grafana, adicione `http://prometheus:9090` como data source. No Kibana, os logs aparecem nos indices `docker-logs-*` depois que o Filebeat e o Logstash comecarem a processa-los.