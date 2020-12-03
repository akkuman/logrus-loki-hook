# logrus-loki-hook
Hook for logrus for Loki


```golang
package main

import (
	"time"

	lokihook "github.com/akkuman/logrus-loki-hook"
	"github.com/sirupsen/logrus"
)

var log = logrus.New()

func init() {
	lokiHookConfig := &lokihook.Config{
		// the loki api url
		URL: "http://admin:admin@loki.xxx.com/api/prom/push",
		// (optional, default: severity) the label's key to distinguish log's level, it will be added to Labels map
		LevelName: "severity",
		// the labels which will be sent to loki, contains the {levelname: level}
		Labels: map[string]string{
			"application": "test",
		},
	}
	hook, err := lokihook.NewHook(lokiHookConfig)
	if err != nil {
		log.Error(err)
	} else {
		log.AddHook(hook)
	}
}

func main() {
	log.Info("I'm a Test.")

	// Because the loki hook use the channel to send log in an asynchronous manner, We should wait for the log to be sent
	time.Sleep(5 * time.Second)
}

```