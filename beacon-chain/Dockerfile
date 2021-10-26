FROM debian:buster-slim as configuration

# Get the image to obtain the binary
RUN apt update && apt install --assume-yes git
RUN git clone https://github.com/openethereum/sbc-prysm-launch /root/sbc

FROM debian:buster-slim 

COPY --from=ghcr.io/openethereum/sbc-prysm-beacon-chain:v2.0.1-sbc /app/cmd/beacon-chain/beacon-chain /usr/local/bin/beacon-chain

# Get the proper configuration files
RUN apt-get update && apt-get install -y wget
RUN wget https://raw.githubusercontent.com/dappnode/DAppNodePackage-Stake-Beacon-Chain/main/node/config/bootnodes.yaml -P /root/sbc/config/ && \
    wget https://raw.githubusercontent.com/dappnode/DAppNodePackage-Stake-Beacon-Chain/main/node/config/config.yml -P /root/sbc/config/

ENTRYPOINT [ "sh", "-c", "exec /usr/local/bin/beacon-chain \ 
    --accept-terms-of-use \
    --contract-deployment-block=\"$DEPLOYMENT_BLOCK\" \
    --http-web3provider=\"$XDAI_RPC_URL\" \
    --bootstrap-node /root/sbc/config/bootnodes.yaml \
    --config-file /root/sbc/config/config.yml \
    --rpc-host=0.0.0.0 \
    --p2p-local-ip=0.0.0.0 \
    --subscribe-all-subnets \
    # --historical-slasher-node \
    # --grpc-gateway-host=0.0.0.0 \
    --monitoring-host=0.0.0.0 \
    --verbosity=debug \
    $EXTRA_OPTS" ]   

