## 祭祖 Proj2 
> ==TODO 交作業要把這 repo 設 public? 

## Note

### Q1 GEM5 + NVMAIN BUILD-UP (40%)
run `./build/X86/gem5.opt configs/example/se.py -c tests/test-progs/hello/bin/x86/linux/hello --cpu-type=TimingSimpleCPU --caches --l2cache --mem-type=NVMainMemory --nvmain-config=../NVmain/Config/PCM_ISSCC_2012_4GB.config`

### Q2 Enable L3 last level cache in GEM5 + NVMAIN (15%)
run `./build/X86/gem5.opt configs/example/se.py -c tests/test-progs/hello/bin/x86/linux/hello --cpu-type=TimingSimpleCPU --caches --l2cache --l3cache --mem-type=NVMainMemory --nvmain-config=../NVmain/Config/PCM_ISSCC_2012_4GB.config`

### Q3 Config last level cache to 2-way and full-way associative cache and test performance (15%)
for 2-way, run `./build/X86/gem5.opt configs/example/se.py -c ./benchmark/quicksort --cpu-type=TimingSimpleCPU --caches --l2cache --l3cache --l3_assoc=2 --l1i_size=32kB --l1d_size=32kB --l2_size=128kB --l3_size=1MB --mem-type=NVMainMemory --nvmain-config=../NVmain/Config/PCM_ISSCC_2012_4GB.config`

for full-wat, run `./build/X86/gem5.opt configs/example/se.py -c ./quicksort --cpu-type=TimingSimpleCPU --caches --l2cache --l3cache --l3_assoc=1 --l1i_size=32kB --l1d_size=32kB --l2_size=128kB --l3_size=1MB --mem-type=NVMainMemory --nvmain-config=../NVmain/Config/PCM_ISSCC_2012_4GB.config`

### Q4 Modify last level cache policy based on RRIP (15%)
run codes (助教會給?)


### Q5 Test the performance of write back and write through policy based on 4-way associative cache with isscc_pcm(15%)
run `./build/X86/gem5.opt configs/example/se.py -c ./benchmark/multiply  --cpu-type=TimingSimpleCPU --caches --l2cache --mem-type=NVMainMemory --nvmain-config=../NVmain/Config/PCM_ISSCC_2012_4GB.config`


### Why dont do 加分題
![](https://i.imgur.com/H9gsGb5.jpg)
