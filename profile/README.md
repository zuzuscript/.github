# ZuzuScript

ZuzuScript is a practical, playful scripting language for the space between shell one-liners and full applications. It is built for automation, data wrangling, command-line tools, small web apps, lightweight GUIs, and readable glue code that can grow from a quick .zzs script into modules, classes, async tasks, and reusable libraries.

The project is intentionally multi-runtime. zuzu-perl, zuzu-rust, and zuzu-js implement the language across Perl, Rust, Node, browser, and Electron contexts; stdlib provides shared modules; languagetests and matrix keep the implementations honest; userguide, examples, and website document and publish the language; and the editor, Android, web console, and designer projects explore where ZuzuScript can run next.

The language itself has a fun mix of familiar and unusual features: typed function signatures, lambdas and closures, classes with generated accessors, rich collection literals, PairLists for duplicate ordered keys, path-query operators for nested data, first-class regexps, async tasks, web request handlers, and GUI widgets. It feels like a scripting language that enjoys being useful.

```javascript
from std/task import all, sleep;

class TrailStop {
      let String name with get;
      let Number sparkle with get := 0;
}

function label_stop ( TrailStop stop ) → String {
      return `${stop.get_name} (${stop.get_sparkle} sparkle)`;
}

function pack_report ( String scout, ... PairList kit ) → String {
      return `${scout} packed ${kit.all("snack").length} snacks and a ${kit{tool}}.`;
}

async function inspect_stop ( TrailStop stop ) {
      await {
              sleep(0.01);
      };

      return {
              name: stop.get_name,
              sparkle: stop.get_sparkle,
      };
}

async function __main__ ( argv ) {
      let route := [
              new TrailStop( name: "Moonlit Bridge", sparkle: 5 ),
              new TrailStop( name: "Old Clocktower", sparkle: 2 ),
              new TrailStop( name: "Secret Bakery", sparkle: 8 ),
      ];

      let favourites := route
              .grep( fn stop → stop.get_sparkle ≥ 5 )
              .map( fn stop → label_stop(stop) );

      let field_notes := {
              scout: {
                      name: "Zia",
                      title: "raccoon field engineer",
              },
              route: favourites,
      };

      say field_notes @ "/scout/name";
      say field_notes @@ "/route/*";

      say pack_report(
              "Zia",
              snack: "berries",
              snack: "biscuits",
              tool: "tiny map",
      );

      let inspections := await {
              all( route.map( fn stop → inspect_stop(stop) ) );
      };

      let total_sparkle := ( inspections @@ "/*/sparkle" ).sum;
      say `Zia logged ${total_sparkle} sparkle before breakfast.`;
}
```
