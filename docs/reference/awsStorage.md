---
title: AWS Storage Considerations
---
<!--
# AWS Storage Considerations

AWS provides many storage options. When considering Ping products deployed in a containerized deployment, the choice typically comes down to two: elastic block storage (EBS) and elastic file system (EFS).  Though there are a number of differences between them, on the surface they act similar when attached to an Elastic Kubernetes Service (EKS) node or Elastic Compute Cloud (EC2) instance.

However, Ping products (whether containerized or not) require high I/O performance, and **Ping only recommends EBS volumes as the backing store**.  EFS performance is significantly lower and is not supported.

For additional product-specific requirements, visit the [appropriate product page](https://docs.pingidentity.com/).
-->

# AWS ストレージに関する考慮事項

AWS は多くのストレージ オプションを提供します。コンテナ化された展開で展開される Ping 製品を検討する場合、選択肢は通常、Elastic Block Storage (EBS) と Elastic File System (EFS) の 2 つになります。これらの間には多くの違いがありますが、Elastic Kubernetes Service (EKS) ノードまたは Elastic Compute Cloud (EC2) インスタンスに接続された場合、表面上は同様に動作します。

ただし、Ping 製品は (コンテナ化されているかどうかに関係なく) 高い I/O パフォーマンスを必要とし、Ping はバッキング ストアとして EBS ボリュームのみを推奨します。 EFS のパフォーマンスは大幅に低下するため、サポートされていません。

その他の製品固有の要件については、[該当する製品ページ](https://docs.pingidentity.com/)をご覧ください。
