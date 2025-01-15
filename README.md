# Animated-Line-Plots-on-R
This is unique code for beginners on R, who wants to visualize their data in an excellent animated way.

# Load required libraries
library(ggplot2)
library(gganimate)
library(readxl)
library(tidyr)

# Load the dataset
data <- read_excel("Environmental_Data_30_Days.xlsx")

# Reshape the data to long format for easier plotting
data_long <- data %>%
  pivot_longer(
    cols = c(`Lights (Lux)`, `CO₂ (ppm)`, `Humidity (%)`, `Temperature (°C)`, `Air Density (kg/m³)`, `Pressure (hPa)`),
    names_to = "Parameter",
    values_to = "Value"
  )

# Create the animated line chart for all parameters
animated_plot <- ggplot(data_long, aes(x = Day, y = Value, color = Parameter, group = Parameter)) +
  geom_line(size = 1) +
  geom_point(size = 2) +
  scale_color_brewer(palette = "Set2") + # Use a distinct color palette
  labs(
    title = "Environmental Parameters over 30 Days",
    x = "Day",
    y = "Values",
    color = "Parameter"
  ) +
  theme_minimal() +
  theme(legend.position = "bottom") +
  transition_reveal(Day)

# Save the animation as a GIF (optional)
anim_save("animated_all_parameters_plot.gif", animation = animate(animated_plot, width = 800, height = 600))

# Display the animation
animate(animated_plot, width = 800, height = 600)
