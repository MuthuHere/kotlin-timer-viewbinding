# kotlin-timer-viewbinding



<div>
<img src="https://user-images.githubusercontent.com/18752533/208798744-b50e266b-6fc1-4ccd-9471-a485c50fc739.jpeg" width="300px">
            </div>


### Enable viewbining

            android {
            ...

              buildFeatures {
                    viewBinding = true
                }

            }


### Service class

          override fun onStartCommand(intent: Intent, flags: Int, startId: Int): Int {
              val time = intent.getDoubleExtra(CURRENT_TIME, 0.0)
              timer.scheduleAtFixedRate(StopWatchTimerTask(time), 0, 1000)
              return START_NOT_STICKY
          }


### StopWatchTimerTask Inner class

          private inner class StopWatchTimerTask(private var time: Double) : TimerTask() {
                override fun run() {
                    val intent = Intent(UPDATED_TIME)
                    time++
                    intent.putExtra(CURRENT_TIME, (time))
                    sendBroadcast(intent)
                }

            }
            
            
### Study about the different 

##### START_STICKY: It will restart the service in case if it terminated and the Intent data which is passed to the onStartCommand() method is NULL. This is suitable for the service which are not executing commands but running independently and waiting for the job.
##### START_NOT_STICKY: It will not restart the service and it is useful for the services which will run periodically. The service will restart only when there are a pending startService() calls. It’s the best option to avoid running a service in case if it is not necessary.
##### START_REDELIVER_INTENT: It’s same as STAR_STICKY and it recreates the service, call onStartCommand() with last intent that was delivered to the service.


### Using broasdcast Receiver

        private val updateTimeBroadcastReceiver = object : BroadcastReceiver() {
              override fun onReceive(context: Context, intent: Intent) {
                  time = intent.getDoubleExtra(StopWatchService.CURRENT_TIME, 0.0)
                  setTime()
              }
          }


### Time calculation

         private fun getFormattedTime(time: Double): String {
                val timeInt = time.roundToInt()
                val hours = timeInt % 86400 / 3600
                val minutes = timeInt % 86400 % 3600 / 60
                val seconds = timeInt % 86400 % 3600 % 60

                return String.format("%02d:%02d:%02d", hours, minutes, seconds)
            }
            
            
            
Happie coding:)            
