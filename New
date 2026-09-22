from pyspark.sql import SparkSession, DataFrame
from pyspark.sql.functions import (
    col,
    when,
    sum as spark_sum,
    to_date
)


# 1. Load Booking Data
def load_bookings_data(spark: SparkSession, path: str) -> DataFrame:
    df = (
        spark.read
        .option("header", True)
        .option("inferSchema", True)
        .csv(path)
    )

    df = df.withColumn(
        "show_date",
        to_date(col("show_date"))
    )

    return df


# 2. Load Movie Reference Data
def load_movie_info(spark: SparkSession, path: str) -> DataFrame:
    df = (
        spark.read
        .option("header", True)
        .option("inferSchema", True)
        .csv(path)
    )

    return df


# 3. Filter Valid Bookings
def filter_valid_bookings(df: DataFrame) -> DataFrame:
    return df.filter(
        (col("seats_booked") >= 0) &
        (col("show_duration_min") >= 0)
    )


# 4. Add Overbooking Breach Flag
def with_overbooking_flag(df: DataFrame) -> DataFrame:
    return df.withColumn(
        "overbooked",
        when(
            col("seats_booked") > col("total_seats"),
            1
        ).otherwise(0)
    )


# 5. Join Movie Metadata
def join_movie_info(
    bookings_df: DataFrame,
    info_df: DataFrame
) -> DataFrame:

    return bookings_df.join(
        info_df,
        on="movie_id",
        how="left"
    )


# 6. Movie Occupancy Efficiency
def movie_occupancy_efficiency(df: DataFrame) -> str:

    grouped_df = (
        df.groupBy(
            "movie_id",
            "total_seats"
        )
        .agg(
            spark_sum("seats_booked").alias("total_booked")
        )
    )

    efficiency_df = grouped_df.withColumn(
        "efficiency",
        when(
            col("total_seats") == 0,
            0.0
        ).otherwise(
            col("total_booked") / col("total_seats")
        )
    )

    result = (
        efficiency_df
        .orderBy(col("efficiency").desc())
        .select("movie_id")
        .first()
    )

    return str(result["movie_id"])
