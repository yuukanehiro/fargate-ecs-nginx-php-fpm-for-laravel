FROM public.ecr.aws/aws-observability/aws-for-fluent-bit:latest

ARG ENVIRONMENT="local" // buildspec-ciで指定する

COPY ./ecs/environments/${ENVIRONMENT}/docker/fluentbit/etc/extra.conf /fluent-bit/etc/extra.conf