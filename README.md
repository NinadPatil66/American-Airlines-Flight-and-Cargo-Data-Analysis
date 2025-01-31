# Analyzing-Flight-Delays-Cancellations-Flight-Duration-Trends
# Import required libraries
import pandas as pd
import matplotlib.pyplot as plt

# Start your code here!
flights2022 = pd.read_csv("flights2022.csv")
flights_weather2022 = pd.read_csv("flights_weather2022.csv")

flights2022['route'] = flights2022['origin'] + "-" + flights2022["dest"]

routes_delays_cancels = flights2022.groupby("route").agg(
    mean_dep_delay=("dep_delay", "mean"),
    total_cancellations=("dep_time", lambda x: x.isna().sum())).reset_index()

top_routes_by_delay = routes_delays_cancels.sort_values('mean_dep_delay',ascending = False).head(9)

top_routes_by_cancellations = routes_delays_cancels.sort_values("total_cancellations", ascending=False).head(9)

print(routes_delays_cancels.columns.tolist())

top9_route_cancels_bar, ax = plt.subplots()
ax.bar(top_routes_by_cancellations['route'],top_routes_by_cancellations['total_cancellations'])
ax.set_xlabel('Route')
ax.set_ylabel('Total Cancellations')
ax.set_title('Routes with Highest Number of Cancellations')
ax.set_xticklabels(top_routes_by_Cancellations["route"], rotation=90)
plt.show()
plt.close()

airlines_delays_cancels = flights2022.groupby('airline').agg(mean_dep_delay = ("dep_delay","mean"),total_cancellations = ("dep_time", lambda x: x.isna().sum())).reset_index()

top_airlines_by_delay = airlines_delays_cancels.sort_values("mean_dep_delay", ascending = False).head(9)

top_airlines_by_cancellations = airlines_delays_cancels.sort_values("total_cancellations", ascending = False).head(9)

top9_airline_delays_bar, ax = plt.subplots()
ax.bar(top_airlines_by_delay['airline'],top_airlines_by_delay['mean_dep_delay'])
ax.set_xlabel('Airline')
ax.set_ylabel('Departure Delay')
ax.set_title('Routes with Highest Mean Departure Delay')
ax.set_xticklabels(top_airlines_by_delay["airline"], rotation=90)
plt.show()
plt.close()

flights_weather2022["group"] = flights_weather2022["wind_gust"].apply(lambda x: ">=10mph" if x>=10 else "<10mph")

wind_grouped_data = flights_weather2022.groupby(["group","origin"]).agg(mean_dep_delay = ("dep_delay","mean"))

print(wind_grouped_data)

wind_response = True




