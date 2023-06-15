# prequesite
- log stash installed
```bash
brew tap elastic/tap
brew install elastic/tap/logstash-full
```
- elastic ready to serve
```
# can get it from git@github.com:s50600822/conductor-cheat.git, under conductor-cheat/es
docker-compose-up up
```

# how to play
![Alt Text](./resources/how_to.gif)
# regexp can be verified
https://regex101.com/r/jvusnc/1

# Ref
##MDC pattern 
	k1=v1, k2=v2
Logback
```java ch.qos.logback.classic.pattern.MDCConverter.java
    private String outputMDCForAllKeys(Map<String, String> mdcPropertyMap) {
        StringBuilder buf = new StringBuilder();
        boolean first = true;
        for (Map.Entry<String, String> entry : mdcPropertyMap.entrySet()) {
            if (first) {
                first = false;
            } else {
                buf.append(", ");
            }
            // format: key0=value0, key1=value1
            buf.append(entry.getKey()).append('=').append(entry.getValue());
        }
        return buf.toString();
    }
```

##log4j
	{{k1=v1},{k2=v2}}
```java org.apache.log4j.pattern.Log4j1MdcPatternConverter
    public void format(final LogEvent event, final StringBuilder toAppendTo) {
        if (key == null) {
            // if there is no additional options, we output every single Key/Value pair for the MDC
            toAppendTo.append('{');
            event.getContextData().forEach(APPEND_EACH, toAppendTo);
            toAppendTo.append('}');
        } else {
            // otherwise they just want a single key output
            final Object val = event.getContextData().getValue(key);
            if (val != null) {
                toAppendTo.append(val);
            }
        }
    }

    private static TriConsumer<String, Object, StringBuilder> APPEND_EACH = new TriConsumer<String, Object, StringBuilder>() {
        @Override
        public void accept(final String key, final Object value, final StringBuilder toAppendTo) {
            toAppendTo.append('{').append(key).append(',').append(value).append('}');
        }
    };
```
