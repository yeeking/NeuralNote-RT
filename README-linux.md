# Instructions for building plugin on Linux

Had a bit of trouble building in linux so storing working script here.


```
# https://github.com/DamRsn/NeuralNote/pull/133
mkdir neural-note
cd neural-note
# clone and sync the magic onnx runtime repo and neural note repo
git clone --recurse-submodules --shallow-submodules  https://github.com/yeeking/NeuralNote-RT.git
git clone https://github.com/polygon/libonnxruntime-neuralnote.git
cd libonnxruntime-neuralnote
git submodule update --init
git checkout linux # switch to linux branch
# replace the non-working make archive script with a working one
cp NeuralNote-RT/make-archive.sh libonnxruntime-neuralnote/
# make penv
python3.10 -m venv venv # 3.11 or lower
source venv/bin/activate
pip install -r requirements.txt
./convert-model-to-ort.sh model.onnx
chmod 755 build-linux.sh
./build-linux.sh model.required_operators_and_types.with_runtime_opt.config
# make edit to comment out check for git tag == 1.15
nano mark-archive.sh
# comment stuff out
./make-archive.sh v1.14.1-neuralnote.0
cd ../
cd NeuralNote-RT/
./build.sh
```


