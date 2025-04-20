head - n 1 tmdb-movies.csv | tr ',' '\n' | nl # xac dinh ten cot
sort -t',' -k16,16r movies.txt > movies_sorted.txt
