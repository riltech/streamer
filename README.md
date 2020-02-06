# streamer
[![Go Report Card](https://goreportcard.com/badge/github.com/riltech/streamer)](https://goreportcard.com/report/github.com/riltech/streamer)
 [![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
 ![GitHub last commit](https://img.shields.io/github/last-commit/riltech/streamer.svg)
 ![GitHub release](https://img.shields.io/github/release/riltech/streamer.svg)

Go Package built around spinning up streaming processes

# How
Stream exposes a struct called `Stream` which starts an underlying ffmpeg process to
transcode incoming raw RTSP stream to HLS based on parameters.

It provides a handy interface to handle streams

[More info on docs](https://godoc.org/github.com/riltech/streamer)

# Example usage

```go
import "github.com/riltech/streamer"

func main() {
	stream, id := streamer.NewStream(
		"rtsp://admin:password@host.dyndns.org:447/Streaming/Channel/2", // URI of raw RTSP stream
		"./videos", // Directory where to store video chunks and indexes. Should exist already
		true, // Indicates if stream should be keeping files after it is stopped or clean the directory
		true, // Indicates if Audio should be enabled or not
		streamer.ProcessLoggingOpts{
			Enabled:    true, // Indicates if process logging is enabled
			Compress:   true, // Indicates if logs should be compressed
			Directory:  "/tmp/logs/stream", // Directory to store logs
			MaxAge:     0, // Max age for a log. 0 is infinite
			MaxBackups: 2, // Maximum backups to keep
			MaxSize:    500, // Maximum size of a log in megabytes
		},
		25*time.Second, // Time to wait before declaring a stream start failed
  )
  
  // Returns a waitGroup where the stream checking the underlying process for a successful start
  stream.Start().Wait() 
}
```
