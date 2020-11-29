# Writeup: Building a Controller #
Brian Hilnbrand

## Implemented body rate control in C++. ##
Body rate control is controlling the rate of correction of the body of the quad.
First I generated motor commands by solving the system of equations to find individual motor thrust commands from the collective thrust command and the desired 3-axis moment. All of these commands were then constrained to the configured min and max motor thrusts.
Next I calculated a desired 3-axis moment given a desired and current body rate. To do so, I calculated the error between the commanded moment and the current moment. I then multiplied this error by the moments of inertia in x/y/z and the gain parameter to find the desired moment command.

## Implement roll pitch control in C++. ##
Next, I implement the roll and pitch control. First I convert the collective thrust command to an acceleration by dividing it by the mass of the quad. Then, I find the roll and pitch commands.
After tuning the banking gain parameter, here is the outcome:

![Attitude Control](./images/attitude_control2.png?raw=true "Attitude Control")

## Implement altitude controller in C++. ##
To implement altitude control, I used a PD controller, first finding the error between commanded z and current z, then multiplying this error by the gain to find the commanded z velocity. This is then constrained to -/+ max descent rate. To find the acceleration command, we find the error between the z velocity command just found and the current z velocity and multiply this with the z velocity gain. We finally calculate thrust and rotate to find thrust required, which is then constrained.

## Implement lateral position control in C++. ##
Here, a simple PD controller is implemented to find the velocity and acceleration commands from the commanded position as well as current pos and velocity. Z component is set to 0 because this position is controlled elsewhere.

## Implement yaw control in C++. ##
Again, a simple PD controller is used to control Yaw. In this case, the yaw error is normalized between 0 and 2$\pi$.

After the altitude, lateral position, and yaw are completed, the result is:

![Position Control](./images/position_control2.png?raw=true "Position Control")

## Non-idealities and robustness. ##
Next, the Altitude controller is updated to account for different mass vehicles by integrating the error over time and multiplying by the gain, which will smooth out the controls. After re-tuning, the different weighted vehicles fly correctly.

![Nonidealities](./images/nonidealities2.png?raw=true "Nonidealities")

After implementing all of these controllers, the vehicles follow the figure 8 trajectory very well!

![Tracking](./images/tracking2.png?raw=true "Tracking")

Note: After submitting my project and receiving feedback about slight errors in my logic, I used [this repo](https://github.com/horagong/FCND-Controls-CPP/) to help debug and follow some logic. I believe that I understand all the concepts now, and this repo was very helpful in debugging.
