Ultimate-Ascent-
================

2012 FRC basic drive/compressor code
#include "WPILib.h"

/**
 * This is a demo program showing the use of the RobotBase class.
 * The SimpleRobot class is the base of a robot application that will automatically call your
 * Autonomous and OperatorControl methods at the right time as controlled by the switches on
 * the driver station or the field controls.
 */ 
#define SOLENOID_HIGH_POWER DoubleSolenoid::kForward
#define SOLENOID_LOW_POWER DoubleSolenoid::kReverse
#define JOYSTICK_LAUNCH 2

class RobotDemo : public SimpleRobot
{
	
	RobotDrive myRobot; // robot drive system
	
	Joystick rightdrivestick; // right joystick
	//Joystick leftdrivestick; // left joystick
	DoubleSolenoid shifterleft;
	Jaguar myRobotShooterFast; //robot shooting system
	Jaguar myRobotShooterSlow;
	Compressor* cPump;
	

public:
	RobotDemo(void):
    myRobot(3,4,1,2),
    rightdrivestick(1),
    //leftdrivestick(2),
    //shifterleft(1,2),
   	myRobotShooterFast(8),
	myRobotShooterSlow(9)
	
	
	
	
{
		myRobot.SetExpiration(0.1);
		cPump = new Compressor(1,1);
		
	}

	/**
	 * Drive left & right motors for 2 seconds then stop
	 */
	void Autonomous(void)
	{
		cPump -> Start();
	}

	/**
	 * Runs the motors with arcade steering. 
	 */
	void OperatorControl(void)
	{
		myRobot.SetSafetyEnabled(true);
		cPump -> Start();
		while (IsOperatorControl())
		{
  if(rightdrivestick.GetRawButton(2))
  {
	 myRobotShooterSlow.Set(0.5);
	 myRobotShooterFast.Set(1.0);
  }
  else
  {
	  myRobotShooterSlow.Set(0.0);
	  myRobotShooterFast.Set(0.0);
  }
			myRobot.TankDrive (rightdrivestick);
			shifterleft.Set(rightdrivestick.GetTrigger() ? SOLENOID_HIGH_POWER : SOLENOID_LOW_POWER);
		//shifterright.Set(rightstick.GetTrigger() ? SOLENOID_HIGH_POWER : SOLENOID_LOW_POWER
	}
	cPump-> Stop();
	}
	/**
	 * Runs during test mode
	 */
	void Test() {
		cPump -> Start();

	}
};

START_ROBOT_CLASS(RobotDemo);
