# Respostas · Avaliação Prática de Docker · ViaSerra Transportes (Turma C)

Nome: Leticia Vito de Oliveira 
Matrícula:26174738
Usuário do GitHub:LeticiaVito
Usuário do Docker Hub: leticiavitodeoliveira99

Responda com as suas palavras e com o que aconteceu na SUA máquina. Resposta curta e certa vale mais
do que texto longo copiado. Resposta que contradiz o seu próprio Dockerfile vale zero.

## Parte 1 · Dockerfile do portal

1. Qual imagem base você usou e qual o tamanho final da imagem do portal (saída de `docker images`)?

Imagem base é nginx:1.27-alpine.
Tamanho (saída do docker images) o que tá no meu computador: DISK USAGE de 73.6MB e CONTENT SIZE de 21MB.

2. Em qual pasta do container o Nginx procura os arquivos do site? Mostre o comando que você usou para
   conferir que o `index.html` está lá dentro.

O Nginx pega os arquivos do site em /usr/share/nginx/html/, então é pra lá que mando o conteúdo da pasta html no COPY. Pra ter certeza que o index.html tava lá dentro, rodei:
docker exec avaliacao-docker-viaserra-portal-1 ls /usr/share/nginx/html
e apareceram o index.html e o estilo.css. Na manutenção eu tinha errado esse caminho, copiei pra /usr/share/nginx/ e acabou aparecendo o "Welcome to nginx!" no lugar da minha página.

## Parte 2 · Docker Hub

3. Nome completo da imagem publicada e link público do repositório no Docker Hub.

O nome da imagem é leticiavitodeoliveira99/viaserra-portal:1.0-26174738. O link público do repositório é https://hub.docker.com/r/leticiavitodeoliveira99/viaserra-portal
4. Se você mudar o HTML, quais comandos precisa rodar para que a versão nova chegue ao Docker Hub?

Primeiro eu construo a imagem de novo com a mesma tag e depois mando pro Docker Hub:
docker build -t leticiavitodeoliveira99/viaserra-portal:1.0-26174738 ./portal
docker push leticiavitodeoliveira99/viaserra-portal:1.0-26174738
Depois ainda faço git add ., git commit e git push pra mudança também ficar no GitHub.

## Parte 3 · Página de manutenção
5. Preencha uma linha por defeito encontrado. Defeito inexistente listado aqui desconta pontos.

| # | Instrução | O que estava errado | O que você viu acontecer | Como corrigiu |
| 1 | `COPY pagina/ .` | a pasta `pagina` não existe, a minha é `site` | o build deu erro `"/pagina": not found` e nem criou a imagem | troquei pra `COPY site/ .` |
| 2 | `CMD ["nginx"]` | o nginx ia pro segundo plano e o container fechava | subiu e uns segundos depois tava `Exited (0)` | coloquei `CMD ["nginx", "-g", "daemon off;"]` |
| 3 | `COPY site/ .` | tava indo pra `/usr/share/nginx/` e não pra pasta `html` | abriu o "Welcome to nginx!" em vez da página laranja | mudei pra `COPY site/ /usr/share/nginx/html/` |

6. Qual a diferença entre `-p 7042:80` e `-p 80:7042` no `docker run`? Qual dos dois números é a porta do container?

No `-p 7042:80`, o 7042 é a porta do meu computador e o 80 é a do container. Acesso `localhost:7042` e cai na porta 80, onde o nginx escuta.
No `-p 80:7042` fica invertido, e o nginx não escuta na 7042. A porta do container é o número depois dos dois pontos (80).
## Parte 4 · Primeiro docker-compose

7. Escreva os dois comandos `docker run` que fariam o mesmo que o seu `docker-compose.yml`.

Portal:
docker run -d --name portal --restart unless-stopped -p 8038:80 leticiavitodeoliveira99/viaserra-portal:1.0-26174738

Manutenção (primeiro construo a imagem):
docker build -t manutencao:26174738 ./manutencao
docker run -d --name manutencao --restart unless-stopped -p 7038:80 manutencao:26174738
8. Qual comando derruba os dois containers de uma vez?

docker compose down
## Verificador

9. Código de conclusão impresso pelo verificador:

VIASERRA-26174738-D562C816
```
(cole aqui)
```
