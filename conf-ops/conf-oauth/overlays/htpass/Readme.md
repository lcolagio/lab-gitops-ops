# Create htpassword

```
echo "Creating htpasswd file"
htpasswd -c -b -B htpasswd admin xxxxxx

htpasswd -b -B htpasswd user1 xxxxxxx
htpasswd -b -B htpasswd user2 xxxxxxx
htpasswd -b -B htpasswd user3 xxxxxxx
htpasswd -b -B htpasswd user4 xxxxxxx

htpasswd -b -B htpasswd dev xxxxxxx
htpasswd -b -B htpasswd qua xxxxxxx
htpasswd -b -B htpasswd prd xxxxxxx
```

```
cat htpasswd | base64 -w 0

YWRtaW46JDJ5JDA1JDlJaHcuMTIxU3M0Q0pQS01palZWck9nV2I2SjdKLmdvblVBTjc4UHd4VVYwZmtLTFg0UUpxCnVzZXIxOiQyeSQwNSROcFZoTE5Wcm9LVC50cHd2RkN3QXFlWS5zcC90TUUxOUdsQTc1cXVhR3FMVnNPYzBDQmdHeQp1c2VyMjokMnkkMDUkTS9GaE1nU1d3NFBUSi5LSXdxQ05KT2NjM05Yb0tJdGxPOTVqdXBaOHBZMWVkc3drM3h1NW0KdXNlcjM6JDJ5JDA1JGZCR1YvQ1R6YU9VTC5JL2pCSklaZnUwTWtKa25JMkpqaGFGV0Fsa1lWVURrMUI5TjVCV1M2CnVzZXI0OiQyeSQwNSR0d1Z1MTNhbkQzTkYyUUpmSGljdjJlYUZ5WUFjLnAvZk5xQ3NxYm56ZWFGZnJoWjhsMzRoLgpkZXY6JDJ5JDA1JG5OMzZHOUJBSzREN3FUcHpEd1ROQk9OODhFclN2WHEyWHZxSC5IRm01WmZ3Ym13NWE4Y2EyCnF1YTokMnkkMDUkamNoT3hzMjVKMlpQOGtCMUtvZGt0ZUU4ZnhZcW81Y0M4ZHZnZTMxekNZTW5hR2RQeXVOMHEKcHJkOiQyeSQwNSQzYjJsMkkuTHhhSmFaMXdSalM4QUtlWGlYSllYLlFjNXpMMXFlQzM5NjRsMmVzcm5rQVJIbQo=

```

Copy it to htpass-secret.yaml secret

