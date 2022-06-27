
mkdir -p ~/.tmp
git clone https://github.com/kubermatic-labs/trainings.git ~/.tmp/trainings
<!-- TODO remove v2 -->
cp -r ~/.tmp/trainings/kkp_fundamentals_v2

choose project in google cloud shell

cd ~/00_cloud_credentials

# set gcp credentials
make get_gcp_sa_key
export GOOGLE_CREDENTIALS=$(cat ~/secrets/key.json)
<!-- TODO maybe move that stuff into secrets folder, also the kubeconfigs -->

# create ssh key pair
<!-- TODO put keys into secret folder? -->
ssh-keygen -N '' -f ~/secrets/kkp_admin_training
eval `ssh-agent`
ssh-add ~/secrets/kkp_admin_training


<!-- TODO use absolute paths triggered from the task dir everywhere -->