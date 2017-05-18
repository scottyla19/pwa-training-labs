# Notes
## Use em Units For:
Any sizing that should scale depending on the font-size of an element other than the root.

Generally speaking, the only reason you’ll need to use em units is to scale an element which has non default font sizing.

As per our example above, design components like menu items, buttons, and headings may have their own explicitly stated font sizes. If you change these font sizes, you want the entire component to scale proportionately.

Common properties this guideline will apply to are margin, padding, width, height and line-height settings, when used on elements with non default font sizing.

I recommend that when you do employ em units, the font size of the element they’re used on should be set in rem units to preserve scalability but avoid inheritance confusion.

## Typically Don’t Use em Units for Font Sizes

It’s quite common to see em units used for font sizing, particularly in headings, however I would suggest that designs are more manageable if rem units are typically used for font sizing.

The reason headings often use em units is they’re a better choice than px units, being relative to regular text size. However rem units can achieve this goal equally well. If any font size adjustment on the html element is made, the heading sizes will still scale too.

## Use rem units for:
Any sizing that doesn’t need em units for the reasons described above, and that should scale depending on browser font size settings.

This accounts for almost everything in a standard design including most heights, most widths, most padding, most margins, border widths, most font sizes, shadows, basically almost every part of your layout.

In a nutshell, everything that can be made scalable with rem units, should be.

## Always Use rem Media Queries

Importantly, when using rem units to create a uniformly scalable design, your media queries should also be in rem units. This will ensure that whatever a user’s browser font size, your media queries will respond to it and adjust your layout.

For example, if a user scales up text very high, your layout may need to snap down from a two columns to a single column, just as it might on a smaller screened mobile device.

If your breakpoints are at fixed pixel widths, only a different viewport size can trigger them. However with rem based breakpoints they will respond to different font sizing too.

## Don’t Use em or rem For:
Multi Column Layout Widths

Column widths in a layout should typically be % based so they can fluidly fit unpredictably sized viewports.
However single columns should still generally incorporate rem values via a max-width setting.
