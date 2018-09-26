# postgis usage example

1. Finding data that coordinate is contained in polygon
   좌표가 폴리곤에 포함되는 데이터 찾기

   ```sql
   SELECT * FROM parcels
   WHERE ST_Contains(POLYGON_DATA, COORDINATE);
   ```

2. Finding data where polygons overlap
   폴리곤이 겹치는 데이터 찾기

   ```sql
   SELECT data FROM parcels A, parcels B
   WHERE ST_DWithin(A.POLYGON_DATA, B.POLYGON_DATA, distance);
   ```


