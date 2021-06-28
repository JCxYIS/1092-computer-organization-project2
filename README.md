# 祭祖 Proj2 Demo Note
### Gem5 + NVmain

## Notes
- debugging: 在 opt 後 py 前加上 `--debug-flags=Cache`

## Q1 GEM5 + NVMAIN BUILD-UP (40%)

### Implementation
照著講義幹過去，Omit

### Demo dodo
```SH
cd gem5

./build/X86/gem5.opt configs/example/se.py -c tests/test-progs/hello/bin/x86/linux/hello \
--cpu-type=TimingSimpleCPU \
--caches \
--l2cache \
--mem-type=NVMainMemory \
--nvmain-config=../NVmain/Config/PCM_ISSCC_2012_4GB.config
```


---


## Q2 Enable L3 last level cache in GEM5 + NVMAIN (15%)

### Implementation
- 改 `configs/common/Caches.py`, `configs/common/Options.py`, `configs/common/CacheConfig.py`：增加 L3Cache
- 改 `src/mem/xbar.py`, `src/cpu/BaseCPU.py`：把 L3 裝上 CPU 
- recompile gem5 (`scons EXTRAS=../NVmain build/X86/gem5.opt`)

### Demo dodo
add `--l3cache`, `--l3_size=XXX` (Optional) to previous command

```sh
./build/X86/gem5.opt configs/example/se.py -c tests/test-progs/hello/bin/x86/linux/hello \
--cpu-type=TimingSimpleCPU \
--caches \
--l2cache \
--l3cache \
--mem-type=NVMainMemory \
--nvmain-config=../NVmain/Config/PCM_ISSCC_2012_4GB.config
```

### Results
水啦！出現了！L3 Cache
![](https://i.imgur.com/X1GavPy.png)
![](https://i.imgur.com/VOPKEBQ.png)

---


## Q3 Config last level cache to 2-way and full-way associative cache and test performance (15%)

### Implementation
- 利用 `--l3_assoc` 覆寫 associativity：2-way 改 2，full-way 則改為 block count 
- 記得 benchmark 要求 cache size 規定 

### Demo dodo
- 2-way 
```sh
./build/X86/gem5.opt configs/example/se.py -c ./benchmark/quicksort \
--cpu-type=TimingSimpleCPU \
--caches \
--l2cache \
--l3cache \
--l3_assoc=2 \
--l1i_size=32kB \
--l1d_size=32kB \
--l2_size=128kB \
--l3_size=1MB \
--mem-type=NVMainMemory \
--nvmain-config=../NVmain/Config/PCM_ISSCC_2012_4GB.config
```
- full-way
```sh
./build/X86/gem5.opt configs/example/se.py -c ./benchmark/quicksort \
--cpu-type=TimingSimpleCPU \
--caches \
--l2cache \
--l3cache \
--l3_assoc=16384 \
--l1i_size=32kB \
--l1d_size=32kB \
--l2_size=128kB \
--l3_size=1MB \
--mem-type=NVMainMemory \
--nvmain-config=../NVmain/Config/PCM_ISSCC_2012_4GB.config
```

### Result
比較：`stats.txt` line 586
![1-way](https://i.imgur.com/6sJN1MX.png)
![2-way](https://i.imgur.com/rNuYulq.png)
![Full-way](https://i.imgur.com/F0iQcmP.png)
> 越多 associativity ，其 miss rate 反而比較高？？


## Q4 Modify last level cache policy based on RRIP (15%)
### Implementation
- 改 `configs/common/Caches.py`，override BaseCache 的 replacement_policy (default: LRU)

### Demo dodo
- compile 助教給的 demo code
- 先跑一次
- 照上面改
- 再跑一次

```
./build/X86/gem5.opt configs/example/se.py -c {} \
--cpu-type=TimingSimpleCPU \
--caches \
--l2cache \
--l3cache \
--l3_assoc=1 \
--l1i_size=32kB \
--l1d_size=32kB \
--l2_size=128kB \
--l3_size=1MB \
--mem-type=NVMainMemory \
--nvmain-config=../NVmain/Config/PCM_ISSCC_2012_4GB.config
```



## Q5 Test the performance of write back and write through policy based on 4-way associative cache with isscc_pcm(15%)
### Implementation
改 `base.cc` 的 access
![](https://i.imgur.com/R7s6x7l.png)

### Demo dodo
```sh
./build/X86/gem5.opt configs/example/se.py  -c ./benchmark/multiply \
--cpu-type=TimingSimpleCPU \
--caches \
--l2cache \
--l3cache \
--l3_assoc=4 \
--l1i_size=32kB \
--l1d_size=32kB \
--l2_size=128kB \
--l3_size=1MB \
--mem-type=NVMainMemory \
--nvmain-config=../NVmain/Config/PCM_ISSCC_2012_4GB.config
```

#### Results
比較：`output` line 413 (and more?)
    
![](https://i.imgur.com/nTFSdQW.png)
> 很多東西都變兩倍大

### 加分題
![](https://i.imgur.com/H9gsGb5.jpg)