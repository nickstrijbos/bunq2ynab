1. Clone the repository:

```sh
git clone git@github.com:wesselt/bunq2ynab.git
```

2. Create a file "config.json" with contents like the one below. You can enter `*` or omit the row entirely to have Bunq2Ynab match accounts by name. Note that if you wildcard the YNAB budget name, one bunq account may end up synching with multiple YNAB accounts in different budgets.

```json
{
  "api_token": "your bunq api key",
  "personal_access_token": "your ynab personal access token",
  "accounts": [
    {
      "bunq_account_name": "your bunq account name",
      "ynab_budget_name": "your ynab budget name",
      "ynab_account_name": "your ynab account name"
    }
  ]
}
```

3. Build the docker container, test and run it

First build the docker image, it does that with de `Dockerfile` in the root folder. Also the docker image starts automatically with the `auto_sync.py` script. So make sure your `config.json` is correct. 
```bash
docker build -t bunq2ynab:latest .
```

After it is done building the image you can try and start it with the command below:
```bash
docker run bunq2ynab:latest 
```

>**warning** Never push your docker image to a public hub/repository! The `config.json` file is copied to your image when building, which has your secrets!

4. Start it with a Docker Compose file

Create a `compose.yaml` file in the root of the bunq2ynab directory. 

It can be as basic as this

```yaml
version: "3.8"

services:
  bunq2ynab:
    build: .
    restart: always
```

After creating start your `compose.yaml` as so, again in the root folder where the file is located.

```bash
docker-compose up -d 
```

it will also build the image and run it! You can remove the `build: .` step if you want to build it on your own. 

## TODO
Mounting the config.json file so the container can be pushed to a registery without secrets. 