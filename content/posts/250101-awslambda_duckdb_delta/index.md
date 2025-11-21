---
title: "Sử dụng AWS Lambda cùng Delta Lake và DuckDB"
weight: 1
draft: false
description: "Code Delta Lake và DuckDB trong Lambda?"
tags: ["Tech", "Data", "AWS"]
# series: ["Documentation"]
series_order: 1
author: ntphiep

---

{{< lead >}}
Dịch từ [AWS Lambda + DuckDB (and Delta Lake)](https://dataengineeringcentral.substack.com/p/aws-lambda-duckdb-and-delta-lake?utm_source=post-email-title&publication_id=1224799&post_id=153591412&utm_campaign=email-post-title&isFreemail=true&r=30kwwb&triedRedirect=true&utm_medium=email)
{{< /lead >}}

Ok thì bài này chủ yếu là mình dịch lại và thêm thắt chút.

Mình nghĩ mình và các bạn đều biết nhiều thuật ngữ dùng để mô tả sự đơn giản trong ngữ cảnh Software Engineering. Tuy nhiên, tiếng còi báo động của modern data stack (ngăn xếp dữ liệu hiện đại ?? :))) đã thu hút nhiều linh hồn tội nghiệp xuống đáy của data sea.

Đừng để chúng ta phải như vậy :))

> Ý tưởng về sự đơn giản trong thiết kế Data Platform, Pipeline, ETLs và dữ liệu nói chung đang rất bị đánh giá thấp. Nó chỉ đơn giản được nói ra nhưng rất ít hoặc không được implement thực tế.


