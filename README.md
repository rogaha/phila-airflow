# phl-airflow

Airflow Configuration for The City of Philadelphia

## Local setup

```bash
# clone the repository
git clone https://github.com/CityOfPhiladelphia/phl-airflow.git
cd phl-airflow

# airflow needs a home, ~/airflow is the default,
# but you can lay foundation somewhere else if you prefer
# (optional)
export AIRFLOW_HOME=`pwd`

# install the project requirements
pip install -r requirements.txt

# initialize the database
airflow initdb

# start the web server, default port is 8080
airflow webserver -p 8080
```

## Dag construction:

'''python

# constructing a new dag requires importing airflow’s DAG class:
from airflow import DAG

# use the DAG class to instantiate a new dag, provide arguments for the name, and default settings. The latter can be created with a dictionary:
#default_args = {
#    'owner': 'airflow',
#    'depends_on_past': False,
#    'start_date': seven_days_ago,
#    'email': ['airflow@airflow.com'],
#    'email_on_failure': True,
#    'email_on_retry': False,
#    'retries': 1,
#    'retry_delay': timedelta(minutes=5),
# }

dag = DAG(‘dag_name’, default_args=default_args)

# create the individual tasks you would like to run:

task1 =  BashOperator(
          task_id ='delete_directory',
          bash_command='''rm -rf ~/path/to/your/directory''',
          dag=dag
        )

success = 'Your Task finished without errors'

task2 = SlackAPIPostOperator(
   task_id='notfiy_success',
   token=Variable.get('slack_token'),
   channel='yourchannel',
   text=success,
   dag=dag
)

task2.execute()

# to set the dependency of one task on another use the convention:

task_name.set_downstream(other_task) - other_task must run after task_name
task_name.set_upstream(another_task) - task_name must run after to another_task

'''
