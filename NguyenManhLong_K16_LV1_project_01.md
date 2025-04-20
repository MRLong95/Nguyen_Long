# xac dinh ten cot
head - n 1 tmdb-movies.csv | tr ',' '\n' | nl # xac dinh ten cot

# Sắp xếp các bộ phim theo ngày phát hành giảm dần rồi lưu ra một file mới
(head -n 1 tmdb-movies.csv && tail -n +2 tmdb-movies.csv | sort -t',' -k14 -r) > sorted_by_release_date.csv

# Lọc ra các bộ phim có đánh giá trung bình trên 7.5 rồi lưu ra một file mới
awk -F',' 'NR==1 || $18 > 7.5' tmdb-movies.csv > top10_movies.csv

# Tìm ra phim nào có doanh thu cao nhất và doanh thu thấp nhất
awk -F',' 'NR==1{next} $5 > max {max=$5; title=$6} END{print "Phim doanh thu cao nhất:", title, max}' tmdb-movies.csv 
awk -F',' 'NR==1{next} ($5 < min || min=="" ) && $5 != "" {min=$5; title=$6} END{print "Phim doanh thu thấp nhất:", title, min}' tmdb-movies.csv
 > low_revenue_movies.csv 

# Tính tổng doanh thu tất cả các bộ phim
awk -F',' 'NR>1 {sum+=$12} END {print "Total revenue: " sum}' tmdb-movies.csv > sum_revenue_movies.csv 

 # Top 10 bộ phim đem về lợi nhuận cao nhất
awk -F',' 'BEGIN {print "profit,revenue,budget,original_title"} NR>1 && $5>0 && $4>0 {profit=$5-$4; print profit","$5","$4","$6}' tmdb-movies.csv | 
sort -t',' -k1 -nr | head -n 10 > top10_profit_movies.csv 

# Đạo diễn nào có nhiều bộ phim nhất
awk -F',' 'NR>1 {split($9, directors, "|"); for (d in directors) print directors[d]}' tmdb-movies.csv | sort | uniq -c | sort -nr | head -n 1 > high_director_movies.csv
# diễn viên nào đóng nhiều phim nhất
awk -F',' 'NR>1 {split($7, actors, "|"); for (a in actors) print actors[a]}' tmdb-movies.csv | sort | uniq -c | sort -nr | head -n 1 > high_actor_movies.csv

 # Thống kê số lượng phim theo các thể loại. Ví dụ có bao nhiêu phim thuộc thể loại Action, bao nhiêu thuộc thể loại Family, ….
awk -F',' 'NR>1 {split($14, genres, "|"); for (i in genres) print genres[i]}' tmdb-movies.csv | sort | uniq -c | sort -nr | column -t -s',' tmdb-movies.csv > genres_movies.csv
