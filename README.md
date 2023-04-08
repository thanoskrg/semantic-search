# semantic-search

This repository is a playground on top of Text Embeddings and Vector Search using Elasticsearch

## Setup

For the following steps you will need [Docker](https://www.docker.com/products/docker-desktop/) installed on your local machine.

### 1. Elasticsearch + Kibana

To spawn an Elasticsearch cluster and a Kibana environment run the following command:

```sh
docker compose up
```

### 2. Start Trial Elasticsearch License

In this case, we are going to run some experiments locally, so the Elasticsearch trial license must be started in order to import our ML models.

In order to do that, open Kibana at http://localhost:5601/app/dev_tools#/console and execute the following query:

```
POST /_license/start_trial?acknowledge=true
```

You should get the following output:

```json
{
  "acknowledged": true,
  "trial_was_started": true,
  "type": "trial"
}
```

### 3. Deploy a text embedding model

First follow the steps as described here on official [`elastic/eland`](https://github.com/elastic/eland#docker) repository.

Then, after you have built the `elastic/eland` docker image, run the following command:

```sh
docker run -it --network host --rm elastic/eland \
  eland_import_hub_model \
    --url http://localhost:9200/ \
    --hub-model-id sentence-transformers/msmarco-MiniLM-L-12-v3 \
    --task-type text_embedding \
    --start
```

After the command has been successfully executed, you should see this on your terminal:

```
Model successfully imported with id 'sentence-transformers__msmarco-minilm-l-12-v3'
```

## Inspiration

The idea to experiment with these technologies was born after reading [How to deploy NLP: Text Embeddings and Vector Search](https://www.elastic.co/blog/how-to-deploy-nlp-text-embeddings-and-vector-search) article from Elastic's official blog.
