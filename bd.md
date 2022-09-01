# Detect examples

### Rapid scan

bash <(curl -s -L https://detect.synopsys.com/detect7.sh) \
--blackduck.url=$URL \
--blackduck.api.token=$TOKEN \
--blackduck.trust.cert=true \
--detect.source.path=$TARGET \
--detect.blackduck.scan.mode=RAPID \
--detect.bom.aggregate.name=aggregated.bdio \
--detect.cleanup=false
