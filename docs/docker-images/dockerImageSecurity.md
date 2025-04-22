---
title: Evaluation of Docker Base Image Security
---
<!--
# Evaluation of Docker Base Image Security

In the Center for Internet Security (CIS) [Docker Benchmark v1.2.0](https://www.cisecurity.org/benchmark/docker/), one of the recommendations says, "4.3 Ensure that unnecessary packages are not installed in the container."

It further states, "You should consider using a minimal base image rather than the standard Red Hat/CentOS/Debian images if you can. Some of the options available include BusyBox and Alpine."

The following sections present security aspects of different Linux distributions compared to Alpine Docker image. This doesn't necessarily mean that one is the best for Docker base images. Other factors, such as usability and compatibility, should also be considered when choosing the most suitable Docker image for an organization.
-->

# Docker Base Imageのセキュリティ評価

Center for Internet Security (CIS) [Docker Benchmark v1.2.0](https://www.cisecurity.org/benchmark/docker/) の推奨事項の 1 つに、「4.3 不要なパッケージがコンテナーにインストールされていないことを確認する」と記載されています。

さらに、「可能であれば、標準の Red Hat/CentOS/Debian イメージではなく、最小限のベース イメージの使用を検討する必要があります。利用可能なオプションには、BusyBox や Alpine などがあります。」とも述べられています。

次のセクションでは、Alpine Docker イメージと比較したさまざまな Linux ディストリビューションのセキュリティ面を示します。これは、必ずしも Docker ベース イメージに最適なものであるという意味ではありません。組織に最適な Docker イメージを選択する場合は、使いやすさや互換性などの他の要素も考慮する必要があります。

<!--
## Evaluation overview

To evaluate Alpine’s security, we compared it with the following popular Linux distributions: Ubuntu, CentOS, and Red Hat Enterprise Linux 7.

For this comparison, we used the latest version, as of March 12, 2020, of each distribution’s Docker image and compared them in four different categories:

* Image size
* Number of packages installed by default
* Number of historical vulnerabilities reported on [cvedetails.com](https://www.cvedetails.com/)
* Number of vulnerabilities reported by the Clair scan

The following table summarizes the numbers for each distribution.

| | Alpine | Ubuntu | CentOS | RHEL7 |
| --- | --- | --- | --- | --- |
| Image Version | alpine:3.11.3 | ubuntu:18.04 | centos:centos8.1.1911 | rhel7:7.7-481 |
| Image Size | 5.59MB | 64.2MB | 237MB | 205MB |
| Number of Packages Installed | 14 | 89 | 173 | 162 |
| Number of Historical CVE*s | 2 | 2007 | 2 | 662 |
| Number of Vulnerabilities Reported by Clair | 0 | 32 | 7 | 0 |

> *CVE - Common Vulnerabilities and Exposures
-->

## 評価の概要

Alpine のセキュリティを評価するために、Ubuntu、CentOS、Red Hat Enterprise Linux 7 という一般的な Linux ディストリビューションと Alpine を比較しました。

この比較では、2020 年 3 月 12 日時点の最新バージョンの各ディストリビューションの Docker イメージを使用し、それらを 4 つの異なるカテゴリで比較しました。

* イメージサイズ
* デフォルトでインストールされるパッケージの数
* [cvedetails.com](https://www.cvedetails.com/) で報告された過去の脆弱性の数
* Clair スキャンによって報告された脆弱性の数

次の表は、各分布の数値をまとめたものです。

| | Alpine | Ubuntu | CentOS | RHEL7 |
| --- | --- | --- | --- | --- |
| イメージバージョン | alpine:3.11.3 | ubuntu:18.04 | centos:centos8.1.1911 | rhel7:7.7-481 |
| イメージサイズ| 5.59MB | 64.2MB | 237MB | 205MB |
| インストールされたパッケージ数 | 14 | 89 | 173 | 162 |
| 過去の CVE* の数 | 2 | 2007 | 2 | 662 |
| Clair によって報告された脆弱性の数 | 0 | 32 | 7 | 0 |

> *CVE - Common Vulnerabilities and Exposures

<!--
## Image size

Alpine has an advantage in image size. Although smaller size doesn’t directly translate into better security, the smaller size does mean less code packed into the image, which means smaller attack surface.
-->

## イメージサイズ

Alpine はイメージサイズで有利です。サイズが小さいことが直接セキュリティの向上につながるわけではありませんが、サイズが小さいということは、イメージに詰め込まれるコードが少なくなり、攻撃対象領域が小さくなることを意味します。

<!--
## Number of packages installed

Because of Alpine's smaller size, Alpine has the fewest packages out of box. Fewer packages means lesser chance of having vulnerabilities in the dependencies, which is a plus for security.
-->

## インストールされているパッケージの数

Alpine はサイズが小さいため、箱から出してすぐに使えるパッケージが最も少ないです。パッケージの数が少ないということは、依存関係に脆弱性が存在する可能性が低いことを意味し、セキュリティにとってはプラスになります。

<!-->
## Number of historical CVEs

Alpine and CentOS both rank highest in number of historical CVEs even though CentOS has a close relationship with RHEL7, and RHEL7 has 600+ reported vulnerabilities.
-->

## 過去の CVE の数

CentOS は RHEL7 と密接な関係があり、RHEL7 には 600 以上の脆弱性が報告されていますが、Alpine と CentOS は両方とも過去の CVE の数で最高位にランクされています。

<!--
## Number of vulnerabilities reported by Clair

Some vulnerabilities reported by Clair might not be real issues, but their presence does mean extra overhead for developers or security teams to triage these findings. This overhead can be avoided if unnecessary dependencies are excluded from the image in the first place.
-->

## Clair によって報告された脆弱性の数

Clair によって報告された一部の脆弱性は実際の問題ではない可能性がありますが、その存在は開発者やセキュリティ チームにとってこれらの発見事項を優先順位付けするために余分なオーバーヘッドを意味します。最初に不要な依存関係がイメージから除外されていれば、このオーバーヘッドを回避できます。

<!--
## Final evaluation results

Although none of the four categories is perfect on its own for evaluating the security of a Linux distribution, in combination, **Alpine** presents greater advantages for use, which is why we selected it as the disribution for all of our Docker images.
-->

## 最終評価結果

4 つのカテゴリはいずれも、単独では Linux ディストリビューションのセキュリティを評価するのに完璧ではありませんが、組み合わせることで **Alpine** を使用するとより大きな利点が得られます。そのため、すべての Docker イメージのディストリビューションとして Alpine を選択しました。

<!--
## References

* [CIS Docker Benchmarks](https://www.cisecurity.org/benchmark/docker/)
* [Alpine CVEs](https://www.cvedetails.com/product/38838/Alpinelinux-Alpine-Linux.html?vendor_id=16697)
* [Ubuntu CVEs](https://www.cvedetails.com/product/20550/Canonical-Ubuntu-Linux.html?vendor_id=4781)
* [CentOS CVEs](https://www.cvedetails.com/product/18131/Centos-Centos.html?vendor_id=10167)
* [Redhat Enterprise Linux CVEs](https://www.cvedetails.com/product/78/Redhat-Enterprise-Linux.html?vendor_id=25)
-->

## 参考文献

* [CIS Docker Benchmarks](https://www.cisecurity.org/benchmark/docker/)
* [Alpine CVEs](https://www.cvedetails.com/product/38838/Alpinelinux-Alpine-Linux.html?vendor_id=16697)
* [Ubuntu CVEs](https://www.cvedetails.com/product/20550/Canonical-Ubuntu-Linux.html?vendor_id=4781)
* [CentOS CVEs](https://www.cvedetails.com/product/18131/Centos-Centos.html?vendor_id=10167)
* [Redhat Enterprise Linux CVEs](https://www.cvedetails.com/product/78/Redhat-Enterprise-Linux.html?vendor_id=25)

<!--
## Ping Identity's Docker Image Hardening Guide

For best practices for securing your product Docker image, see Ping Identity's [Hardening Guide](https://support.pingidentity.com/s/article/Docker-Image-Hardening-Deployment-Guide).
-->

## Ping Identity の Docker イメージ強化ガイド

製品の Docker イメージを保護するためのベスト プラクティスについては、Ping Identity の[強化ガイド](https://support.pingidentity.com/s/article/Docker-Image-Hardening-Deployment-Guide)を参照してください。
