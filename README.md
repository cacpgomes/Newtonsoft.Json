Newtonsoft.Json
===============

Json.NET is a popular high-performance JSON framework for .NET

Add a convenience class to convert a Json token String or Array to IList<String>. I needed because the Json I parsed is not always well-formed: sometimes is an array but if there's only on element, it's a string.
An implementation of the class:

        internal class EventContractResolver : DefaultContractResolver
        {
            internal static readonly EventContractResolver Instance = new EventContractResolver();

            protected override JsonProperty CreateProperty(System.Reflection.MemberInfo member, MemberSerialization memberSerialization)
            {
                JsonProperty property = base.CreateProperty(member, memberSerialization);

                if (property.DeclaringType.Equals(typeof(Tags)))
                {
                    if(property.PropertyName == "tag") {
                        property.MemberConverter = new StringOrArrayToIListJsonConverter();
                    }
                }
                return property;
            }
        }
