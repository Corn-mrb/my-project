1. SSH로 Umbrel 서버 접속
ssh umbrel@<umbrel-IP>

2. 설정 파일 저장할 디렉토리 생성(SD카드/업그래드시 삭제되지않는 위치)
   mkdir -p /mnt/data/upgrades/charge-lnd

3. 설정파일 작성
   sudo nano /mnt/data/upgrades/charge-lnd/charge.config

4. config 수정 명령어
   sudo nano /mnt/data/upgrades/charge-lnd/charge.config

5. Charge-lnd 실행

   ocker run --rm -it --network=umbrel_main_network \
-e GRPC_LOCATION=10.21.21.9:10009 \
-e LND_DIR=/data/.lnd \
-e CONFIG_LOCATION=/app/charge.config \
-e TLS_CERT_PATH=/data/.lnd/tls.cert \
-e MACAROON_PATH=/data/.lnd/data/chain/bitcoin/mainnet/admin.macaroon \
-v /home/umbrel/umbrel/app-data/lightning/data/lnd:/data/.lnd \
-v /mnt/data/upgrades/charge-lnd/charge.config:/app/charge.config \
accumulator/charge-lnd:latest

6. config 초기화
   sudo truncate -s 0 /mnt/data/upgrades/charge-lnd/charge.config

7. crontab 넣기
   crontab -e

8 1시간 타이머
0 * * * * docker run --rm -e GRPC_LOCATION=10.21.21.9:10009 -e LND_DIR=/data/.lnd -e CONFIG_LOCATION=/app/charge.config -e TLS_CERT_PATH=/data/.lnd/tls.cert -e MACAROON_PATH=/data/.lnd/data/chain/bitcoin/mainnet/admin.macaroon -v /home/umbrel/umbrel/app-data/lightning/data/lnd:/data/.lnd -v /mnt/data/upgrades/charge-lnd/charge.config:/app/charge.config accumulator/charge-lnd:latest >> /home/umbrel/charge-lnd.log 2>&1

