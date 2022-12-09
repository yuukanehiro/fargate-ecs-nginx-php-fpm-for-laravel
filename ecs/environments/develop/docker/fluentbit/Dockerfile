FROM public.ecr.aws/aws-observability/aws-for-fluent-bit:latest

ARG ENVIRONMENT="local" // override by CI/CD! ["develop"|"staging"|"production"]

COPY ./ecs/environments/${ENVIRONMENT}/docker/fluentbit/etc/extra.conf /fluent-bit/etc/extra.conf