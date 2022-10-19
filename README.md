 S3 compatibility tests
========================

This is a set of unofficial Amazon AWS S3 compatibility tests, that can be useful to people implementing software that exposes an S3-like API. The tests use the Boto3 library.

## Usage

### 1. Install

#### 1.1 Download the source code from GitHub.

```shell
git clone https://github.com/sine-io/s3tests-sineio.git
```

#### 1.2 Create virtual environment if needed

```shell
# If use anaconda, you can execute the commands as follows
# create virtual env
conda create -n s3tests-sineio python=python3

# activate virtual env
conda activate s3tests-sineio

cd s3tests-sineio

# install the requirements
pip install -r requirements.txt
# or
pip install -r requirements.txt -i https://pypi.douban.com/simple
```

> The tests use the `python3, pytest` test framework.

#### 1.3 Copy s3tests.conf and pytest.ini

##### 1.3.1 s3tests.conf

You will need to create a configuration file with the location of the service and two different credentials. A sample configuration file named `s3tests.conf.SAMPLE` has been provided in this repo.

```shell
cd s3tests

cp s3tests.conf.SAMPLE s3tests.conf
```

Once you have that file copied and renamed `s3tests.conf`, You should edited `host, port, ssh_user, ssh_passwd, ssh_port`

```shell
[DEFAULT]
## this section is just used for host, port and bucket_prefix
host = xx.xx.xx.xx
## S3 port
port = 7480
## username, password, port for ssh connection
## sun node of the cluster is better.
ssh_user = xxx
ssh_passwd = xxx
ssh_port = 22
```

> 1. host is IP of the S3 endpoint's IP
> 2. port is S3 endpoint's port
> 3. ssh_user is server user
> 4. ssh_passwd is server password
> 5. ssh_port is server port to login via ssh

If your storage is `Ceph` or `Ceph based`, you can create users as follows:

    python bootstrap_sio.py

If your storage is others, you should create two users manually.

```shell
# Main User:
# user id and displayname: mainuser
# email: mainuser@example.com
# access_key: main
# secret_key: main

# Alt User:
# user id and displayname: altuser
# email: altuser@example.com
# access_key: alt
# secret_key: alt
```

##### 1.3.2 pytest.ini

Once you have that file copied and renamed `pytest.ini`, You should edited `rp_*` if use `reportportal`(for the Test reports). If you do not want to use reportportal, you can let it be or delete it.

```shell
cp pytest.ini.SAMPLE pytest.ini

# change the items as follows:
# rp_uuid is user token
rp_uuid = xxxxx
# rp_endpoint is reportportal URL
rp_endpoint = http://ip:port
# rp_project is reportportal project in your user
rp_project = xxx
# rp_launch is Testing name
rp_launch = s3api-testing
# rp_launch_description is Testing description
rp_launch_description = S3 Api Automation Test
# rp_launch_attributes is attributes
rp_launch_attributes = 's3api' 'stage:testing' 'version:2.4.0.4'
# rp_issue_system_url is Bug tracker(e.g: Jira)
rp_issue_system_url = http://xxxx?m=bug&f=view&bugID={issue_id}
```

#### 2. Run the tests

You can run the tests with(dir is `s3tests-sineio/s3tests`):

```shell
pytest or pytest --s3cfg s3tests.conf-path
```

You can specify which test to run:

```shell
pytest -k EXPRESSION
e.g. :
    pytest -k test_bucket_list_empty
    pytest -k 'not test_bucket_list_empty'
```

Some tests have attributes set based on their current reliability and things like AWS not enforcing their spec strictly. You can filter tests based on their attributes:

```shell
pytest -m "sio and not lifecycle_need_speedup" -n 10 --reruns 3
# or
pytest tests -m "sio and not lifecycle_need_speedup" -n 10 --reruns 3 --reportportal
```

You can run the tests via Shell scripts

```shell
sh run.sh
# or
sh run_without_rp.sh
```

Report generated by `pytest-html` is in the report directory:

```shell
s3tests-sineio
    |___ s3tests
            |___ report  # <--- here.
            |___ tests
# Open it via chrome or firefox.
```

Report generated by `reportportal` is on the reportportal webpage. Open the url, then you will see it.

## Contribution

There are many different ways to contribute to S3TESTS-SINEIO's development, just find the one that best fits with your skills. Examples of contributions we would love to receive include:

- Code patches
- Bug reports
- Patch reviews
- and so on
