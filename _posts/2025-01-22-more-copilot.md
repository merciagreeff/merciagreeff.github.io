---
title: "More Copilot in Visual Studio Code"
layout: post
categories: code
---

Today I started off again by entering a formula for Mohr's Circle of Stress, though the formula was not described completely.

![2025-01-22-1.png](/assets/2025-01-22-1.png)

I asked Copilot to plot the circle. It correctly identified it as Mohr's Circle and filled in the gaps. I asked it to add principal stresses, shear forces and stresses on a plane with a specified angle. I also made suggestions to improve the graphics, like shifting some of the text around.  A few iterations later this was the final result.  No coding from my side.

![2025-01-22-2.png](/assets/2025-01-22-2.png)

```
import math
import matplotlib.pyplot as plt

def calculate_sigma_x_prime(sigma_x, sigma_y, tau_xy, theta):
    theta_rad = math.radians(theta)
    sigma_x_prime = (sigma_x + sigma_y) / 2 + (sigma_x - sigma_y) / 2 * math.cos(2 * theta_rad) + tau_xy * math.sin(2 * theta_rad)
    return sigma_x_prime

def calculate_tau_x_prime_y_prime(sigma_x, sigma_y, tau_xy, theta):
    theta_rad = math.radians(theta)
    tau_x_prime_y_prime = - (sigma_x - sigma_y) / 2 * math.sin(2 * theta_rad) + tau_xy * math.cos(2 * theta_rad)
    return tau_x_prime_y_prime

def plot_stress_block(ax, stress_block_x, stress_block_y):
    ax.plot(stress_block_x, stress_block_y, 'k-')
    ax.set_title('Stress Block')
    ax.set_xlabel('X')
    ax.set_ylabel('Y')
    ax.grid(True)
    
# Example usage
sigma_x = 100  # example value
sigma_y = 50   # example value
tau_xy = 25    # example value

# Plot the stress block
plot_stress_block(ax, stress_block_x, stress_block_y)

# Calculate the stresses for the given angle
sigma_x_prime = calculate_sigma_x_prime(sigma_x, sigma_y, tau_xy, theta)
tau_x_prime_y_prime = calculate_tau_x_prime_y_prime(sigma_x, sigma_y, tau_xy, theta)

# Plot the Mohr circle with the given variables, principal stresses, shear stresses, and stresses for the given angle
def plot_mohr_circle_with_angle(center, radius, sigma_x, sigma_y, tau_xy, sigma_1, sigma_2, theta, sigma_x_prime, tau_x_prime_y_prime):
    # Create the figure and axis
    fig, ax = plt.subplots(figsize=(10, 10))

    # Plot the Mohr circle
    circle = plt.Circle((center, 0), radius, fill=False, color='b', linestyle='-')
    ax.add_artist(circle)

    # Plot the center of the circle
    ax.plot(center, 0, 'bo')

    # Plot the points representing the given state of stress
    ax.plot(sigma_x, tau_xy, 'ro')
    ax.plot(sigma_y, -tau_xy, 'ro')

    # Plot the principal stresses
    ax.plot(sigma_1, 0, 'go')
    ax.plot(sigma_2, 0, 'go')

    # Plot the maximum and minimum shear stresses
    max_shear_stress = radius
    min_shear_stress = -radius
    ax.plot(center, max_shear_stress, 'mo')
    ax.plot(center, min_shear_stress, 'mo')

    # Plot the stresses for the given angle
    ax.plot(sigma_x_prime, tau_x_prime_y_prime, 'co')

    # Label the points
    ax.text(sigma_x, tau_xy, f'  (σx={sigma_x}, τxy={tau_xy}) at θ=0°', verticalalignment='bottom', horizontalalignment='left')
    ax.text(sigma_y, -tau_xy, f'  (σy={sigma_y}, -τxy={-tau_xy}) at θ=0°', verticalalignment='top', horizontalalignment='left')
    ax.text(center, 0, f'  Center: ({center}, 0)', verticalalignment='bottom', horizontalalignment='right')
    ax.text(sigma_1, 0, f'  σ1: {sigma_1:.2f}', verticalalignment='bottom', horizontalalignment='left')
    ax.text(sigma_2, 0, f'  σ2: {sigma_2:.2f}', verticalalignment='bottom', horizontalalignment='right')
    ax.text(center, max_shear_stress, f'  τmax: {max_shear_stress:.2f}', verticalalignment='bottom', horizontalalignment='left')
    ax.text(center, min_shear_stress, f'  τmin: {min_shear_stress:.2f}', verticalalignment='top', horizontalalignment='left')
    ax.text(sigma_x_prime, tau_x_prime_y_prime, f'  (σx\'={sigma_x_prime:.2f}, τx\'y\'={tau_x_prime_y_prime:.2f}) at θ={theta}°', verticalalignment='bottom', horizontalalignment='left')

    # Draw a line with an arrow from the center to the perimeter of the circle to indicate the radius
    ax.annotate('', xy=(center + radius / math.sqrt(2), -radius / math.sqrt(2)), xytext=(center, 0),
                arrowprops=dict(facecolor='black', shrink=0.05, width=0.5, headwidth=5))

    # Indicate the radius
    ax.text(center + radius / (2 * math.sqrt(2)), -radius / (2 * math.sqrt(2)), f'Radius: {radius:.2f}', 
            verticalalignment='bottom', horizontalalignment='left')

    # Set the labels and title
    ax.set_xlabel('Normal Stress (σ)')
    ax.set_ylabel('Shear Stress (τ)')
    ax.set_title('Mohr Circle with Principal and Shear Stresses')

    # Set the aspect of the plot to be equal
    ax.set_aspect('equal', 'box')

    # Set the limits of the plot
    ax.set_xlim(center - radius - 40, center + radius + 40)
    ax.set_ylim(-radius - 40, radius + 40)

    # Set the grid
    ax.grid(True)

    # Show the plot
    plt.show()

# Plot the Mohr circle with the given variables, principal stresses, shear stresses, and stresses for the given angle
plot_mohr_circle_with_angle(center, radius, sigma_x, sigma_y, tau_xy, sigma_1, sigma_2, theta, sigma_x_prime, tau_x_prime_y_prime)
```

I then added formulas for a and b for the equation above. It took a bit for the autocomplete to correctly predict what I wanted for a, then it got there much quicker for b.

![2025-01-22-3.png](/assets/2025-01-22-3.png)
![2025-01-22-4.png](/assets/2025-01-22-4.png)

Autopilot selected the following values, though theta = 45 came from another example in my notebook.

![2025-01-22-5.png](/assets/2025-01-22-5.png)

Afterwards I quickly checked if the calculated values were actually correct, and it probably needs further checks to ensure everything is working properly otherwise.  

Either way, it was quite an impressive time saver! 

Any comments are welcome at [https://github.com/merciagreeff/merciagreeff.github.io/issues/7](https://github.com/merciagreeff/merciagreeff.github.io/issues/7) :)