### 查询 Template 中使用的 Media 资源列表

```sql
SELECT 
  r.template_content, 
  r.resource_id, 
  r.resource_version, 
  r.type
FROM 
  template.template_resource_ref AS r
WHERE 
  r.type = 'F' OR r.type = 'M'
```

### 查询下架 media 时候的相关信息

```sql
select 
  m.id, 
  m.brand, 
  m.type, 
  mb.version 
from 
  media as m 
inner join 
  media_bundle as mb 
on 
  m._id = mb.media
```

### 媒体下架 - 查询媒体的缩略图(仅限于 Global)

```sql
SELECT 
  t.media_id,
  'https://' || CAST(json_extract(t.files, '$["0-thumbnail"].bucket') AS VARCHAR) || '/' || CAST(json_extract(t.files, '$["0-thumbnail"].key') AS VARCHAR) as media_thumbnail_url,
  t.media_type, 
  t.version, 
  t.brand_id
FROM 
  model.dim_media_library AS t limit 10;
```

### 媒体下架 - 查询媒体的缩略图(可以在 cn 使用)

```sql
SELECT
  m.id as media_id,
  m.type as media_type,
  b.version as media_version,
  b.brand as brand_id,
  f.bucket as media_bucket, 
  f.key_ as media_key
FROM
  media.media as m
inner JOIN
  media.media_bundle as b
ON 
  m._id = b.media 
INNER join 
  media.media_file as f
on 
  b._id = f.media_bundle
WHERE
  f.quality_code = 'T' AND
  m.id IN (
    'MADIXKg0eMQ'
  )
```