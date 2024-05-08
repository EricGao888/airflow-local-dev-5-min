# airflow-local-dev-5-min
Set up airflow local dev env in 5 minutes - This is not an official instruction. This is only for Chinese mainland developers 
who have network issues when installing python and js dependencies.

### 2024-05-06
#### version
*  main (dev branch)
*  2.9.0
*  Airflow uses `hatch` to manage local dev env since 2.8.1

#### backend
1. go to the airflow project root path
2. run `hatch python install 3.10`, from the error output, find the path of hatch venv.
3. `source <hatch-venv-path>/bin/activate`
4. `pip install httpx[socks]`
5. open vpn and set network proxy in terminal
```shell
export http_proxy=<your-proxy-address>
export https_proxy=<your-proxy-address>
```
5. `hatch env show`
6. find a env and run hatch env create airflow-310
7. close vpn and unset proxy in terminal
8. `hatch env find airflow-310`
9. run `hatch -e airflow-310 shell` or `source the airflow-310 venv`
10. run `pip install -e ".[devel]"` - before running pip install, run `pip config list -v` and make sure the registry is set to domestic registry
11. (optional) set the alias in ~/.zshrc a="xxxxx/bin/airflow"  (default airflow is not the one installed), if path contains blank, use '' outside "", e.g. - alias a='"/Users/alibaba/Library/Application Support/hatch/env/virtual/apache-airflow/Hq9asqcX/airflow-310/bin/airflow"' and `source ~/.zshrc`
12. run `hatch -e airflow-310 shell` or source the airflow-310 venv to get into venv, and run `a standalone`, run `a db reset` if there is existing standalone db file

#### front-end
1. run `yarn config set registry https://registry.npmmirror.com`, change the registry to domestic registry
2. delete `yarn.lock`, otherwise the registry will not take effect
3. delete all yarn / npm proxies -
```shell
yarn config delete proxy
yarn config delete https-proxy
npm config delete proxy
npm config delete https-proxy
```
5. run `yarn config list` to make sure all proxies deleted
6. run `pre-commit run --hook-stage manual compile-www-assets --all-files` to compile front-end assets
