# Sagemaker-Rapids

This short document is intented as a quick install guide for those interested in using Rapids within Sagemaker.

1) Be sure you are using a p3 instance (v100 GPU). p2 instances are no good because K80 cards are not supported to be used with Rapids.

2) Once the instance is started use the terminal to execute the following:
    
    *source /home/ec2-user/anaconda3/etc/profile.d/conda.sh*
    
    *conda create --name rapids python=3.6*
    
    *conda activate rapids*
    

    I strongly recommend to create a new environment because if you try to install Rapids in the default python3 environment it will take a lot of time to solve the environment and also it won't be able to find everything it need to be compatible.


3) In the new environment conda install Rapids and some other packages that will be required to read s3 files:
    
    *conda install -c rapidsai -c nvidia -c numba -c conda-forge -c anaconda cudf=0.9 cuml=0.9 cugraph=0.9 dask-cudf=0.9 python=3.6 \
        ipykernel boto3 "PyYAML>=3.10,<4.3" "urllib3<1.25,>=1.21" "idna<2.8,>=2.5" boto s3fs dask anaconda::cudatoolkit=10.0*
    

    It will take about 12 minutes to solve the environment.
    
 
 4) Install sagemaker and register the environment:
      
      *pip install sagemaker*
      
      *ipython kernel install --user --name=rapids*
      
      
 
 5) After a minute or so you may change the kernel in jupyter lab: kernel -> change kernel -> rapids
 
 
 6) In jupyter, we do a basic test:
      
      *import cudf*
      
      *import cuml*
      
      *import dask*
      
      *import pandas as pd*
      
      *import dask_cudf*
      
      
      *df = cudf.DataFrame()*
      
      *df['key'] = [0, 1, 2, 3, 4]*
      
      *df['val'] = [float(i + 10) for i in range(5)]*
      
      *print(df)*
      
      
      *df_float = cudf.DataFrame()*
      
      *df_float['0'] = [1.0, 2.0, 5.0]*
      
      *df_float['1'] = [4.0, 2.0, 1.0]*
      
      *df_float['2'] = [4.0, 2.0, 1.0]*
      
      *dbscan_float = cuml.DBSCAN(eps=1.0, min_samples=1)*
      
      *dbscan_float.fit(df_float)*
      
      *print(dbscan_float.labels_)*
      
      

7) If you are using Sagemaker it's very probably your data is on s3. To read data: 
      
      *import boto3*
      
      *import sagemaker*
      
      *from sagemaker import get_execution_role*
      
      *role = get_execution_role()*
      
      
      *df=dask_cudf.read_parquet('s3://XXX-YYY-ZZZ.snappy.parquet', compression='snapy')*
      *len(df)*
      

 
 8) To be continued...
