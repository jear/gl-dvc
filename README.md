# gl-dvc

# Create your bucket here : http://192.168.201.73:9090/buckets   ( redacted / redacted ) 

# From your  Juputer nb in GL MLOps  https://gwtest.gwtest.com:10006/ , 
# Open a Terminal
# source ~/.bashrc

# You should have already made you conda init …. 

# Add your Kaggle creds in ~/.bashrc
export KAGGLE_PROXY=http://172.28.6.17:3128
export KAGGLE_USERNAME=redacted
export KAGGLE_KEY='redacted'

source ~/.bashrc

conda create -n dvc python=3.8
conda activate dvc

pip install "dvc[s3]"
pip install determined-cli
pip install kaggle

mkdir -p gl-dvc/images
cd gl-dvc/images
kaggle competitions download -c dogs-vs-cats
Downloading dogs-vs-cats.zip to /home/jerome.armand@hpe.com/gl-dvc/images

# unzip … rename …

(dvc) bash-4.2$ dvc init

(dvc) bash-4.2$ dvc remote add -d minio-dvc-bucket s3://dvc-bucket -f
Setting 'minio-dvc-bucket' as a default remote.
(dvc) bash-4.2$ dvc remote modify minio-dvc-bucket endpointurl http://192.168.201.71
(dvc) bash-4.2$ dvc remote modify minio-dvc-bucket access_key_id <redacted>
(dvc) bash-4.2$ dvc remote modify minio-dvc-bucket secret_access_key <redacted>
(dvc) bash-4.2$ dvc push -r minio-dvc-bucket --all-commits --all-tags --all-branches
37394 files pushed   

(dvc) bash-4.2$ cat .dvc/.gitignore 
/config.local
/tmp
/cache

(dvc) bash-4.2$ cat .dvcignore 
# Add patterns of files dvc should ignore, which could improve
# the performance. Learn more at
# https://dvc.org/doc/user-guide/dvcignore

(dvc) bash-4.2$ cat .gitignore 
/images

