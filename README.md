## ClamAV Signature Databases
    
Mirror of ClamAV database files. Intended as a reliable source so that systems using out-of-date versions can easily download the signature databases using common command line tools like `git`, `wget`, or `curl`.
    
__Commands to update database files.__
    
```bash
rm -f main.cvd daily.cld daily.cvd bytecode.cvd main.cvd.sha256 daily.cvd.sha256 bytecode.cvd.sha256 main.cvd.* daily.cvd.* && freshclam --user $(id -n -u) --datadir=$(pwd) --config-file=$(pwd)/freshclam.conf && split --numeric-suffixes=01 --number=10 main.cvd main.cvd. && split --numeric-suffixes=01 --number=10 daily.cvd daily.cvd. && sha256sum main.cvd &> main.cvd.sha256 && sha256sum daily.cvd &> daily.cvd.sha256 && sha256sum bytecode.cvd &> bytecode.cvd.sha256 && sha256sum -c main.cvd.sha256 daily.cvd.sha256 bytecode.cvd.sha256 &> /dev/null && [ "$(git status -s --untracked-files=no -- main.cvd.[0-1][0-9] main.cvd.sha256 daily.cvd.[0-1][0-9] daily.cvd.sha256 bytecode.cvd bytecode.cvd.sha256 | wc -l)" -ne "0" ] && { git add main.cvd.[0-1][0-9] main.cvd.sha256 daily.cvd.[0-1][0-9] daily.cvd.sha256 bytecode.cvd bytecode.cvd.sha256 && git commit -m "Database Files Updated / $(date --utc +%Y%m%d-%H%M) UTC" && git push ; } ; git checkout -- main.cvd.[0-1][0-9] main.cvd.sha256 daily.cvd.[0-1][0-9] daily.cvd.sha256 bytecode.cvd bytecode.cvd.sha256
```
    
__Commands to download and recombine the database files.__
    
```bash
rm -f main.cvd daily.cld daily.cvd bytecode.cvd main.cvd.sha256 daily.cvd.sha256 bytecode.cvd.sha256 main.cvd.* daily.cvd.* && curl -LSOs https://github.com/ladar/clamav-data/raw/main/main.cvd.[01-10] -LSOs https://github.com/ladar/clamav-data/raw/main/main.cvd.sha256 -LSOs https://github.com/ladar/clamav-data/raw/main/daily.cvd.[01-10] -LSOs https://github.com/ladar/clamav-data/raw/main/daily.cvd.sha256 -LSOs https://github.com/ladar/clamav-data/raw/main/bytecode.cvd -LSOs https://github.com/ladar/clamav-data/raw/main/bytecode.cvd.sha256 && cat main.cvd.01 main.cvd.02 main.cvd.03 main.cvd.04 main.cvd.05 main.cvd.06 main.cvd.07 main.cvd.08 main.cvd.09 main.cvd.10 > main.cvd && cat daily.cvd.01 daily.cvd.02 daily.cvd.03 daily.cvd.04 daily.cvd.05 daily.cvd.06 daily.cvd.07 daily.cvd.08 daily.cvd.09 daily.cvd.10 > daily.cvd && sha256sum -c main.cvd.sha256 daily.cvd.sha256 bytecode.cvd.sha256 || { printf "ClamAV database download failed.\n" ; rm -f main.cvd daily.cvd bytecode.cvd ; } ; rm -f main.cvd.sha256 daily.cvd.sha256 bytecode.cvd.sha256 main.cvd.* daily.cvd.* 
```

