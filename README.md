# dify-model-lb

二次开发dify社区版打开模型负载均衡功能

任何修改请自行参考不同版本许可证协议并咨询官方，本up主只做技术探讨，所讨论功能也和本人供职公司无关，相关版权问题概不负责

本仓库为AI带路党Pro视频 [如何在Dify社区版打开企业版的模型负载均衡功能-Dify二次开发](https://www.bilibili.com/video/BV192rnY4EmD/) 的配套说明

修改源码 api\services\feature_service.py
```python
class FeatureService:

    @classmethod
    def get_features(cls, tenant_id: str) -> FeatureModel:
        features = FeatureModel()

        cls._fulfill_params_from_env(features)

        if dify_config.BILLING_ENABLED:
            cls._fulfill_params_from_billing_api(features, tenant_id)
        # 新增此行
        features.model_load_balancing_enabled = True

        return features
```

```
# API service
api:
build: ../api
restart: always
environment:
# Use the shared environment variables.
<<: *shared-api-worker-env
# Startup mode, 'api' starts the API server.
MODE: api
depends_on:
- db
- redis
volumes:
# Mount the storage directory to the container, for storing user files.
- ./volumes/app/storage:/app/api/storage
networks:
- ssrf_proxy_network
- default

给你参考，docker-compose文件修改下，然后重新build
以后我在出个视频教下怎么部署
```
