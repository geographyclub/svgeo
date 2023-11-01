# svgeo

Geographic files converted to svg format -- ready to use with any vector editing software.

## Downloads

[ne_50m_admin_0_countries](https://github.com/geographyclub/svgeo/tree/main/ne_50m_admin_0_countries)

## Conversion

Converting Natural Earth layer to individual svg files using GDAL/OGR shell script.

```
table=ne_50m_admin_0_countries
height=400
width=400
#mkdir ${table}
#rm ${table}/*
ogrinfo -so natural_earth_vector.gpkg | grep "${table}" | sed -e 's/^.*: //g' -e 's/ (.*$//g' | while read layer; do
  ogrinfo -dialect sqlite -sql "SELECT name || CAST(X'09' AS TEXT) || ST_MinX(geom) || CAST(X'09' AS TEXT) || (-1 * ST_MaxY(geom)) || CAST(X'09' AS TEXT) || (ST_MaxX(geom) - ST_MinX(geom)) || CAST(X'09' AS TEXT) || (ST_MaxY(geom) - ST_MinY(geom)) || CAST(X'09' AS TEXT) || AsSVG(geom, 1, 3) FROM ${layer}" natural_earth_vector.gpkg | grep -e '=' | sed -e 's/^.*://g' -e 's/^.* = //g' | while IFS=$'\t' read -a array; do
    name=$(echo ${array[0]} | sed -e 's/ /_/g' -e "s/'//g" -e 's/\.//g')
    echo '<svg xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" height="'${height}'" width="'${width}'" viewBox="'${array[1]}' '${array[2]}' '${array[3]}' '${array[4]}'"><path d="'${array[5]}'" vector-effect="non-scaling-stroke" fill="#000" fill-opacity="1" stroke="#FFF" stroke-width="0px" stroke-linejoin="round" stroke-linecap="round"/></svg>' > ${table}/${name}.svg
  done
done
```
